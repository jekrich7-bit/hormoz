# 6. Biznes, moliya, huquq va risklar

## 6.1. Taklif (offer): qiymat tenglamasi

Qiymat = (Orzu natija × Erishish ehtimoli) / (Vaqt × Kuch)

| Omil | Studio | Motion |
|---|---|---|
| **Orzu natija** ↑ | "Kino sifatidagi video, kamerasiz va jamoasiz" | "Professional reklama video, dizaynersiz" |
| **Erishish ehtimoli** ↑ | Explore'dagi yuzlab real namunalar. "Natija chiqmasa kredit qaytadi" | 30+ namuna, mahalliy shablonlar, mijozlar sharhlari |
| **Vaqt** ↓ | Bir necha daqiqa | Agentlikda **3-7 kun**, bizda **2 daqiqa** |
| **Kuch** ↓ | Rasm yuklab presetni bosish kifoya | O'zbek tilida bitta gap yozish kifoya |

**Kafolat (risk reversal):** "Generatsiya muvaffaqiyatsiz bo'lsa, kredit avtomatik qaytadi" (bu tizimda allaqachon bor). Bunga qo'shimcha: "Birinchi Motion videongiz yoqmasa, kreditni qaytaramiz."

**Narxni ankerlash:** agentlikdagi oddiy motion video narxi bilan solishtiring (mahalliy bozordan haqiqiy narxlarni yig'ing, odatda bu millionlab so'm). Bizda esa taxminan 10 000 so'm.

**Bonuslar (to'plam):** brend to'plami, 50+ shablon, musiqa kutubxonasi, o'zbekcha ovoz va subtitrlar. Raqobatchilarda bularning har biri alohida pul turadi.

## 6.2. Tariflar (umumiy kredit, ikkala mahsulot uchun)

> 1 kredit ≈ 1 000 so'm ro'yxat narxida. Kurs 1$ ≈ 12 500 so'm deb olingan. Ishga tushirishdan oldin joriy kursni qo'ying.

| Tarif | Narx | Kredit | 1 kredit narxi | Taxminan nima yetadi |
|---|---|---|---|---|
| **Bepul** | 0 | 10 (bir marta) | | Bir nechta rasm va 1 ta Lite video yoki 1 ta Motion video |
| **Start** | 99 000 so'm | 100 | 990 so'm | 4 ta Veo Fast video **yoki** 10 ta Motion video |
| **Pro** | 249 000 so'm | 270 | 922 so'm | Veo Standard ochiladi, navbatda ustuvorlik |
| **Studio** | 599 000 so'm | 700 | 856 so'm | 4K, watermarksiz, jamoa (v3) |
| **Agency** (v3) | kelishiladi | 2 000+ | | API, bulk, bir nechta o'rindiq |
| Qo'shimcha paket | 50 000 so'm | 50 | 1 000 so'm | Muddati 12 oy |

- MVP'da **avtomatik obunasiz** boshlang: oylik paket sotiladi, kreditlar 30 kun amal qiladi, tugashidan oldin Telegram yoki email orqali eslatma yuboriladi. Avtomatik yechish (karta tokenizatsiyasi) keyinroq qo'shiladi.
- Xalqaro versiyada narx USD'da bo'ladi va boshqa raqamlar bilan belgilanadi (masalan $12, $29, $69).

### Marja tekshiruvi (Studio, eng yomon holat: barcha kreditlar video uchun sarflansa)

```
1 kreditning tannarxi = 0.08 $ / 2.5 (markup) = 0.032 $ ≈ 400 so'm
Start:  990 so'm sotiladi, tannarxi 400 so'm → yalpi marja ≈ 60%
Studio: 856 so'm sotiladi, tannarxi 400 so'm → yalpi marja ≈ 53%
To'lov komissiyasi (~1-2%) ayriladi. Ishlatilmay qolgan kreditlar marjani oshiradi.
Motion'da tannarx ancha past, shuning uchun marja 75-85% bo'ladi.
```

## 6.3. Boshlang'ich byudjet (birinchi 3 oy, taxminiy)

| Modda | Oyiga | 3 oyga |
|---|---|---|
| Hosting (Vercel Pro, DB, Trigger.dev, Upstash, R2) | $100-250 | $300-750 |
| AI provayderlar (test, beta, bepul kreditlar) | $200-500 | $600-1 500 |
| AWS (Remotion Lambda) | $10-50 | $30-150 |
| Domenlar, SMS, email | $30-60 | $90-180 |
| Musiqa litsenziyasi (bir martalik) | | $100-300 |
| Marketing testlari (Instagram/Telegram reklama, bloggerlar) | $300-700 | $900-2 100 |
| Yuridik (MChJ, oferta, yurist maslahati) | | mahalliy narxlar bo'yicha |
| **Jami** | | **~$2 000-5 000** |

**Pul oqimi qoidasi:** foydalanuvchi pulni **oldindan** to'laydi (kredit sotib oladi), provayderga esa keyin to'laysiz. Bepul tarif qattiq cheklangan bo'lsa, katta pul yo'qotish xavfi yo'q.

## 6.4. Daromad stsenariysi (namuna, kafolat emas)

| Oy | Studio pullik foydalanuvchilar | Motion pullik foydalanuvchilar | O'rtacha chek | MRR (taxminan) |
|---|---|---|---|---|
| 2 | 30 | | 150 000 so'm | 4.5 mln so'm (~$360) |
| 4 | 120 | 60 | 170 000 so'm | 30 mln so'm (~$2 400) |
| 6 | 300 | 200 | 180 000 so'm | 90 mln so'm (~$7 200) |
| 12 | 800 | 900 + B2B | 200 000 so'm | 340 mln+ so'm (~$27 000+) |

Asosiy richaglar: aktivatsiya (birinchi natija tezligi), presetlar va shablonlarning viral ta'siri, Telegram kanali va B2B (agentliklar).

## 6.5. Huquqiy masalalar (yurist bilan tasdiqlang)

1. **Yuridik shaxs:** MChJ. To'lov tizimlari va B2B shartnomalar uchun kerak.
2. **IT Park rezidentligi:** soliq imtiyozlari bor (manbalarga ko'ra foyda solig'i, QQS va ijtimoiy soliqdan ozod qilish, xodimlar uchun 7.5% JShDS). Imtiyozlar muddati va shartlarini (2028 yoki 2040, eksport ulushi) IT Park saytidan yoki soliq maslahatchisidan aniqlang.
3. **Shaxsiy ma'lumotlar:** "Shaxsga doir ma'lumotlar to'g'risida"gi qonunga **2026-yil 26-martda** o'zgartirish kiritildi. Avvalgi "hammasi O'zbekistonda saqlansin" talabi yumshatildi:
   - **Biometrik va genetik ma'lumotlar** hamon faqat O'zbekiston hududidagi serverlarda saqlanishi kerak. **Characters funksiyasidagi yuz rasmlari** shunga kirishi mumkin.
   - Boshqa ma'lumotlarni chet elda saqlash mumkin, agar shartlardan biri bajarilsa (adekvat davlat, standart shartnoma bandlari va boshqalar). 2026-avgustda 49 ta davlat soddalashtirilgan ro'yxatga kiritildi.
   - **Amaliy xulosa:** asosiy baza (email, telefon, to'lovlar) adekvat davlatda bo'lishi mumkin. Characters uchun yuz rasmlarini O'zbekistondagi hostingda saqlashni yoki bu funksiyani yurist tasdiqlaganidan keyin ishga tushirishni rejalashtiring.
4. **Oferta va foydalanish shartlari:** kreditlar qaytarilishi, muddatlar, taqiqlangan kontent, AI kontentini belgilash va natijalarga bo'lgan huquqlar.
5. **Mualliflik huquqi:** musiqa faqat litsenziyali bo'lsin, stok rasmlarni litsenziyasi ruxsat bergan holda ishlating. Foydalanuvchi yuklagan logo va brendlar uchun "huquqim bor" tasdig'ini oling.
6. **Provayder shartlari:** har bir model provayderining (Google, Kling, ByteDance) foydalanish siyosati sizning foydalanuvchilaringizga ham taalluqli. Ularni o'z ToS'ingizga kiriting.

## 6.6. Risklar va ularni kamaytirish

| # | Risk | Ehtimol | Ta'sir | Choralar |
|---|---|---|---|---|
| 1 | Provayder narxni oshiradi yoki modelni yopadi (Sora misoli) | Yuqori | Yuqori | Adapterlar, bazadagi katalog, fallback modellar, markup admin paneldan o'zgaradi |
| 2 | Bepul tarifni suiiste'mol qilish va soxta akkauntlar | Yuqori | O'rta | Telefon tasdig'i, bepul videoni cheklash, fingerprint, rate limit |
| 3 | Deepfake va noqonuniy kontent | O'rta | Yuqori | 3 qatlamli moderatsiya, ToS, log, bloklash, mashhur shaxslar taqiqlangan |
| 4 | Valyuta riski (xarajat USD'da, daromad so'mda) | O'rta | O'rta | Kredit qiymatini har chorakda kursga moslash, 15-20% bufer |
| 5 | To'lov tizimlariga ulanish kechikadi | Yuqori | O'rta | Faza 0'da ariza berish. Vaqtincha qo'lda to'lov va Telegram Stars |
| 6 | Katta raqobatchilar (Google, Higgsfield, Claude Motion) | Yuqori | O'rta | Mahalliy niche, til, narx, Telegram, tezlik, B2B munosabatlar |
| 7 | Yolg'iz ishlash (charchash, bus factor) | O'rta | Yuqori | Kichik scope, avtomatlashtirish, scope'ni qisqartirish qoidasi (5-hujjat) |
| 8 | Provayderning ishlamay qolishi yoki limitlari | O'rta | O'rta | Navbat, retry, fallback, status sahifa, foydalanuvchiga ochiq xabar |
| 9 | Biometrik ma'lumotlar qonuni | O'rta | Yuqori | Yurist, lokal saqlash, rozilik, Characters'ni kechiktirish |
| 10 | Remotion litsenziya narxi o'sishi | Past | Past | Engine abstraksiyasi, zaxira sifatida HyperFrames |
| 11 | LLM sifati beqaror (Motion) | O'rta | O'rta | Eval to'plami, sxema validatsiyasi, deterministik composer |

## 6.7. Marketing va kanallar (go-to-market)

1. **Dogfooding kontenti:** har kuni platformada yaratilgan 3-5 ta video Instagram Reels, TikTok, YouTube Shorts va Telegram kanaliga joylanadi. Har bir video "shu 2 daqiqada qilindi" deb ko'rsatiladi.
2. **Telegram:**
   - o'z kanalingiz (yangi presetlar, namunalar, aksiyalar);
   - SMM va biznes kanallari bilan hamkorlik va reklama;
   - Faza 5'dan keyin bot o'zi ham kanal bo'ladi ("botga post yubor, video ol").
3. **Affiliate:** SMM kurslari o'qituvchilari va bloggerlar uchun 20-30% komissiya (promo kod orqali).
4. **Referral:** ikkala tomonga 30 kreditdan.
5. **Jamoalar:** Uzum Market sotuvchilari guruhlari, SMM hamjamiyatlari, IT Park tadbirlari.
6. **O'zbekcha YouTube darsliklari:** "Higgsfield'ning o'zbekcha muqobili", "2 daqiqada reklama video" kabi mavzular. SEO va ishonch uchun kerak.
7. **B2B:** 20 ta agentlik va 20 ta biznesga shaxsan demo, "Agency" tarifi va shaxsiy onboarding.
8. **Xalqaro:** build-in-public (X, LinkedIn), Product Hunt va Motion'ning ingliz tilidagi versiyasi.

## Manbalar

- Shaxsiy ma'lumotlar qonuni: [Kun.uz (2026-03-27)](https://kun.uz/en/news/2026/03/27/uzbekistan-amends-personal-data-law-to-facilitate-global-payment-systems), [Kun.uz (2026-08-04)](https://kun.uz/en/news/2026/08/04/uzbekistan-approves-49-countries-for-simplified-personal-data-transfers), [Times of Central Asia](https://timesca.com/uzbekistan-eases-data-localization-rules-to-support-global-payment-platforms/), [Legal500](https://www.legal500.com/developments/thought-leadership/legislative-updates-in-the-field-of-artificial-intelligence-and-personal-data-regulation-uzbekistan/)
- IT Park imtiyozlari: [IT Park](https://www.it-park.uz/en/itpark/news/tax-incentives-for-startups-and-foreign-investors-in-uzbekistan-s-it-sector), [EY Uzbekistan](https://www.ey.com/en_uz/technical/tax-alerts/2024/10/ey-uz-decree-on-additional-incentives-to-support-it-park-residents)
- To'lovlar: [sayt.uz: PayPal/Stripe va O'zbekiston](https://sayt.uz/en/blog/paypal-stripe-uzbekistan), [tolov (PyPI): Payme/Click/Uzum SDK](https://pypi.org/project/tolov/), [Dodo Payments: Uzbekistan](https://dodopayments.com/payments-in/uzbekistan), [fungies.io: LemonSqueezy alternativalari](https://fungies.io/lemonsqueezy-alternatives-2/)
- Remotion narxlari: [remotion.dev/docs/license/pricing](https://www.remotion.dev/docs/license/pricing)
