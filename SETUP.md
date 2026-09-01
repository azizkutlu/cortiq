# Cortiq — ödeme kurulumu (Lemon Squeezy)

Site statik (GitHub Pages). Ödeme, Lemon Squeezy'nin **Merchant of Record**
altyapısıyla alınır: vergiyi/KDV'yi onlar yönetir, sana net ödeme yapar.

Front-end tarafı hazır ve **kapalı bir bayrakla** duruyor. Aşağıdaki adımları
yapıp `index.html` içindeki `PAY` bloğunu doldurunca canlıya alınır.

---

## 1. Lemon Squeezy hesabı + ürün (senin yapacağın)

1. https://app.lemonsqueezy.com → hesap aç (bireysel olabilir, şirket şart değil).
2. **Settings → Payouts**: ödeme yöntemini bağla (PayPal veya Wise).
3. **Store** oluştur (örn. "Cortiq").
4. **Products → New Product**:
   - İsim: `Cortiq IQ Sonucu`
   - Pricing: **Single payment** (tek seferlik), fiyatı belirle (örn. 2.99 USD ya da TRY karşılığı)
   - Dijital ürün, teslimat gerektirmez
5. Ürünü **Publish** et, **"Share / Buy link"** URL'sini kopyala:
   `https://<store>.lemonsqueezy.com/buy/xxxxxxxx`

## 2. Front-end'i aç (bende/sende — tek dosya)

`index.html` içindeki `PAY` bloğunu güncelle:

```js
const PAY={
  enabled:true,                                            // aç
  checkoutUrl:'https://<store>.lemonsqueezy.com/buy/xxxxxxxx',
  priceLabel:'₺99',                                        // Lemon Squeezy fiyatıyla AYNI olsun
  verifyUrl:''                                             // adım 3'ü yapmazsan boş bırak
};
```

Bu kadarı **para almaya yeter.** Kullanıcı öder, Lemon Squeezy "başarılı" der,
sonuç açılır. (Not: sonuç tarayıcıda üretildiği için teknik bir kullanıcı
teorik olarak ödemeden de açabilir — küçük bir azınlık; gerçek koruma için
adım 3.)

## 3. (Opsiyonel) Sunucu doğrulaması — Cloudflare Worker

Ödemeyi sunucuda doğrulamak ve ileride puanı sunucudan üretmek istersen:

1. https://dash.cloudflare.com → ücretsiz hesap → **Workers & Pages → Create Worker**
2. Aşağıdaki kodu yapıştır, `LEMON_API_KEY`'i **Settings → Variables → Secret**
   olarak ekle (Lemon Squeezy → Settings → API'den alınır).
3. Worker URL'sini (`https://cortiq-pay.<sub>.workers.dev`) `PAY.verifyUrl`'e
   `.../verify` şeklinde yaz.

```js
// Cloudflare Worker — Lemon Squeezy sipariş doğrulama
export default {
  async fetch(req, env) {
    const origin = req.headers.get('Origin') || '*';
    const cors = {
      'Access-Control-Allow-Origin': origin,
      'Access-Control-Allow-Methods': 'GET,OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type',
    };
    if (req.method === 'OPTIONS') return new Response(null, { headers: cors });

    const url = new URL(req.url);
    if (!url.pathname.endsWith('/verify')) {
      return new Response('not found', { status: 404, headers: cors });
    }
    const id = url.searchParams.get('order_id');
    if (!id) return json({ ok: false, error: 'order_id yok' }, 400, cors);

    const r = await fetch(`https://api.lemonsqueezy.com/v1/orders/${id}`, {
      headers: {
        Accept: 'application/vnd.api+json',
        Authorization: `Bearer ${env.LEMON_API_KEY}`,
      },
    });
    if (!r.ok) return json({ ok: false, error: 'siparis bulunamadi' }, 404, cors);
    const data = await r.json();
    const paid = data?.data?.attributes?.status === 'paid';
    return json({ ok: paid }, 200, cors);
  },
};
function json(obj, status, cors) {
  return new Response(JSON.stringify(obj), {
    status,
    headers: { 'Content-Type': 'application/json', ...cors },
  });
}
```

---

## Sonraki sertleştirme adımları (ileride)
- Puanı ve sonuç kartını Worker'dan, ödeme doğrulandıktan sonra üretmek
  (tam koruma).
- Lemon Squeezy **webhook**'u ile satışları bir tabloya kaydetmek (raporlama).
- Reklamdan gelen trafiği ölçmek için `utm` + basit bir analytics.
