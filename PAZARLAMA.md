# Cortiq — reklam hazırlığı

İlk etap: **Instagram (Meta Ads)** + **Google arama reklamı**.
Site tarafındaki ölçüm kodu hazır — sadece ID'leri gir (aşağıda).

---

## 0. Ölçüm ID'lerini gir (site hazır, kapalı bekliyor)

`index.html` içindeki `TRACK` bloğu:

```js
const TRACK={
  metaPixelId:'',   // Meta → Events Manager → Pixel ID (sadece rakam)
  googleTagId:''    // Google Ads: 'AW-XXXXXXXXXX'  ·  GA4: 'G-XXXXXXXXXX'
};
```

Site şu 3 olayı otomatik gönderir (reklam optimizasyonu bunlara göre öğrenir):

| Adım | Meta olayı | Google olayı |
|---|---|---|
| Test başladı | `Lead` | `test_start` |
| Ödeme duvarı görüldü | `InitiateCheckout` | `reach_paywall` |
| Satın alma (ödeme sonrası) | `Purchase` (değer+para birimi) | `purchase` |

> Google Ads'te "Dönüşüm" olarak **purchase**'ı seç; Meta'da kampanya hedefi **Satış / Purchase** olsun. Böylece platform "parayı getireni" bulmaya çalışır.

---

## 1. Meta Ads (Instagram / Facebook)

**Kampanya hedefi:** Satış (Sales) → Dönüşüm: Purchase
**Yerleşim:** Reels + Stories + Keşfet (Advantage+ placements açık)
**Kitle (başlangıç):** Türkiye, 18–45, geniş bırak (Meta pixel'den öğrensin). İlgi alanı eklemek istersen: bulmaca, zeka oyunları, Mensa, sudoku.
**Günlük bütçe:** 150–300 TL, 2–3 reklam kreatifiyle başla.

### Reklam metinleri (Meta)

**Kreatif A — meydan okuma**
- Birincil metin: `Bu 20 soruyu 3 dakikada çözebilen çok az insan var. IQ puanın kaç çıkacak? 👇`
- Başlık: `IQ'nu 3 dakikada test et`
- Açıklama: `20 görsel mantık sorusu · anında sonuç`
- CTA: **Learn More / Daha Fazla**

**Kreatif B — merak**
- Birincil metin: `Ortalama IQ 100. Sen ortalamanın üstünde misin? 20 soruluk hızlı testle öğren.`
- Başlık: `Gerçek IQ puanın kaç?`
- Açıklama: `Hemen başla, 3 dakika sürer`

**Kreatif C — sosyal kanıt / sayı**
- Birincil metin: `Arkadaşların çözemedi. Sen çözebilecek misin? 🧠 20 soru, 200 saniye.`
- Başlık: `20 soruda IQ testi`
- Açıklama: `Süreye karşı yarış`

> Video fikri (Reels): ekranda 1 soru göster ("sıradaki sayı?"), 3 saniye geri sayım, "cevabı ve IQ'nu öğren → linke tıkla". Bu format hem reklamda hem organik Keşfet'te çalışır.

---

## 2. Google Ads (Arama)

**Kampanya türü:** Search (Arama)
**Hedef:** Dönüşüm = purchase
**Dil/Konum:** Türkçe, Türkiye
**Günlük bütçe:** 100–200 TL

### Anahtar kelimeler

Öbek eşleme (phrase) ile başla, sonra veriye göre daralt:

```
"iq testi"
"iq test"
"zeka testi"
"online iq testi"
"iq testi çöz"
"gerçek iq testi"
"iq ölçme testi"
"iq testi ücretsiz"
"zeka seviyesi testi"
"iq hesaplama"
"iq testi 2026"
"mensa iq testi"
```

**Negatif kelimeler** (boş tıklama yakma): `ödev, çocuk, pdf, indir, kitap, iş ilanı, wisc, meb`

### Reklam metni (Responsive Search Ad)

**Başlıklar (30 karaktere kadar, 8–10 tane gir):**
```
IQ Testi – 3 Dakikada
Gerçek IQ Puanını Öğren
20 Soruluk Online IQ Testi
Zekanı Süreye Karşı Test Et
Anında IQ Sonucu
IQ'n Ortalamanın Üstünde mi?
Görsel Mantık IQ Testi
Hemen Başla, 3 Dakika
```

**Açıklamalar (90 karaktere kadar, 4 tane):**
```
20 görsel mantık sorusu, anında sonuç. IQ puanın ve yüzdelik dilimin hazır.
Kayıt yok, hemen başla. Süreye karşı 20 soru, gerçek IQ tahmini.
Puan + kategori analizi + paylaşılabilir sonuç kartı. Tek seferlik, aboneliksiz.
Zekanı ölç: örüntü, mantık, uzamsal ve sayısal akıl yürütme.
```

**Görünen URL yolu:** `/iq-testi`

---

## 3. Sıra
1. Hesapları aç (Instagram işletme + Facebook sayfası + Meta Business + Google Ads)
2. Pixel ID + Google tag ID'yi bana ver → siteye gireyim, canlıya alayım
3. Yukarıdaki kreatif/metinleri gir, günlük küçük bütçeyle başlat
4. 3–4 gün veri topla → ödeme başına maliyeti (CPA) en düşük olanı büyüt
