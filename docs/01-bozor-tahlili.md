# 1. Bozor tahlili

> Manbalar hujjat oxirida. Ko'p ma'lumot uchinchi tomon sharhlaridan olingan, shuning uchun narx va funksiyalarni rasmiy saytdan qayta tekshiring.

## 1.1. Google Flow nima qiladi

Google'ning AI kino studiyasi (2025-yil may oyida chiqqan). Veo (video), Imagen/Nano Banana (rasm) va Gemini (matn) ustiga qurilgan.

| Funksiya | Mohiyati | Biz uchun xulosa |
|---|---|---|
| **Scene Builder** | Kliplarni ketma-ket ulab, uzluksiz sahna yasaydi | Loyiha va timeline kerak (Studio v2) |
| **Ingredients to Video** | 3 tagacha referens rasm (personaj, obyekt, joy) bilan bir xil ko'rinishni saqlaydi | "Personajlar" funksiyasi (Studio v2) |
| **Frames to Video** | Birinchi va oxirgi kadrni berasiz, model o'rtasini to'ldiradi | O'tishlar uchun kerak |
| **Extend** | Klipni oxirgi kadridan davom ettiradi | Uzun video yasash usuli |
| **Kamera boshqaruvi** | Pan, tilt, zoom va boshqalar | Kamera presetlari (Studio MVP) |
| **Native audio** | Veo 3.1 ovoz, effekt va dialogni o'zi generatsiya qiladi | Audio bor/yo'q modellarni katalogda farqlash |
| **Obyekt qo'shish/olib tashlash** | Mavjud videoni tahrirlash | Keyingi bosqich |
| **Flow Agent** (2026, Gemini asosida) | Ko'p qadamli rejalashtirish va loyiha xotirasi | Bizda ham "Director" agent bo'ladi |
| **Kirish** | Google AI Plus/Pro/Ultra obunasi, oylik kredit limiti | Pullik obuna + kredit modeli bozorda odatiy holga aylangan |

**Kuchli tomoni:** Veo sifati va Google ekotizimi. **Zaif tomoni (biz uchun imkoniyat):** o'zbekcha interfeys yo'q, so'mda to'lab bo'lmaydi, faqat Google modellari ishlatiladi, ijtimoiy tarmoq (Reels/TikTok) uchun tayyor shablonlar kam.

## 1.2. Higgsfield nima qiladi

Higgsfield **agregator**: o'z modelini emas, boshqa kompaniyalarning modellarini (Seedance, Kling, Wan, Veo 3.1, Sora 2 va boshqalar) bitta interfeysda kredit bilan sotadi. Ustiga o'z vositalarini qurgan.

| Funksiya | Mohiyati |
|---|---|
| **Cinema Studio** | Kino uslubidagi kamera harakatlari va kadr boshqaruvi |
| **Soul ID** | Foydalanuvchi o'z yuzini o'rgatadi va keyin har xil sahnalarda o'zini ko'radi |
| **Speak** | Gapiruvchi avatar, lip-sync |
| **Presetlar / "Apps"** | Bir bosishda ishlaydigan viral effektlar. Ichida tayyor prompt, model va parametrlar bor. **Asosiy o'sish dvigateli shu** |
| **Kredit tizimi** | Har model har xil kredit oladi. Oddiy video 15-25 kredit, premium (Veo 3.1) 40-70 kredit. Kreditlar keyingi oyga o'tmaydi |

**Narxlar (2026-oktabr boshi, uchinchi tomon ma'lumoti):** Free $0, Starter $19/oy (~270 kredit), Plus $59/oy (~1 200 kredit), Ultra $129/oy (~3 000 kredit). Qo'shimcha kredit taxminan $5 / 100 kredit.

**Asosiy saboq:** Higgsfield model sifati bilan emas, **tayyor presetlar + marketing + kredit iqtisodiyoti** bilan yutadi. Presetni yaratish arzon (u shunchaki konfiguratsiya), lekin har bir yangi preset yangi viral kontent va yangi foydalanuvchi olib keladi. Biz ham xuddi shunday "preset dvigateli" quramiz.

## 1.3. Claude Motion: Platforma B uchun to'g'ridan-to'g'ri raqobatchi

Anthropic **2026-yil 8-oktabrda** Claude Motion'ni (beta) chiqardi:
- Matn, grafik, shakl va rasmlardan **qisqa animatsion explainer** yasaydi.
- **Video generatsiya modeli ishlatmaydi.** Claude animatsiyani kod sifatida yozadi. Shuning uchun bitta so'z, raqam yoki vaqtni o'zgartirsangiz, qolgan qismi o'zgarmaydi.
- Real odam yoki real footage yaratmaydi.
- Faqat **Team va Enterprise** tariflarida mavjud.
- Hamkorlar ro'yxatida (natijani ochib davom ettirish mumkin bo'lgan joylar) Runway, Higgsfield, HeyGen, Adobe, Descript, invideo va Luma bor.

**Xulosa:**
1. Bu **bozorni tasdiqlaydi**: "kod orqali motion-grafika" yondashuvi to'g'ri, katta kompaniya ham shu yo'ldan bordi.
2. Biz bilan to'qnashuv: Claude Motion asosan korporativ explainer'larga qaratilgan. Biz esa **SMM, reklama, Reels/TikTok formati, o'zbek va rus tilidagi ovoz, mahalliy shablonlar, so'mdagi arzon narx va Telegram**ni olamiz. Bularning hech biri Claude Motion'ning maqsadi emas.
3. Bizning tezlik va'damiz ("2 daqiqada tayyor MP4") Claude Code bilan qo'lda qilishdan ham, umumiy explainer vositalaridan ham aniq ajralib turishi kerak.

## 1.4. Boshqa o'yinchilar

| Toifa | O'yinchilar | Izoh |
|---|---|---|
| AI video agregatorlar | Krea, Freepik, OpenArt, Leonardo, Pollo, Pixverse | Bozor to'yingan, farq UX va narxda |
| Model egalari (o'z ilovasi bilan) | Kling, Runway, Luma, Hailuo (MiniMax), Vidu | API orqali biz ularning modellarini ham ishlatamiz |
| Motion-grafika / animatsiya | Canva, CapCut, Vidu motion tool, VideoGen, Vidio.ai, Pexo | Ko'pi shablon asosida, AI direktor zaif |
| Ochiq kodli "video = kod" | **Remotion** (React), **HyperFrames** (HeyGen, Apache 2.0, HTML asosida, 2026-aprel) | Bizning render qatlamimiz shular ustiga quriladi |

## 1.5. Bozordagi muhim signal: Sora yopildi

- OpenAI 2026-yil 24-martda Sora'ni to'xtatishini e'lon qildi. Ilova 26-aprelda, **API esa 24-sentabrda** yopildi. O'rniga boshqa model berilmadi.
- **Saboq:** bitta provayderga bog'langan biznes bir kunda yopilib qolishi mumkin. Shuning uchun bizda:
  - har bir model **adapter** orqali ulanadi;
  - model katalogi **bazada** saqlanadi (deploy qilmasdan o'chirish/almashtirish mumkin);
  - har bir preset uchun **zaxira model** (fallback) belgilanadi.

## 1.6. Raqobatchilardagi bo'shliqlar (bizning imkoniyat)

1. **Til.** O'zbek tilida (lotin va kirill) interfeys, prompt va ovoz yo'q. Rus tili ham ko'pincha zaif.
2. **To'lov.** Ko'p foydalanuvchida xalqaro karta yo'q yoki undan foydalanish noqulay. Payme, Click va Uzum orqali to'lash muhim ustunlik beradi.
3. **Narx.** Dollar narxlari mahalliy xarid qobiliyatiga mos emas. So'mdagi kichik paketlar kerak.
4. **Telegram.** O'zbekistonda asosiy platforma shu, lekin raqobatchilarda bot yoki Mini App yo'q.
5. **Mahalliy shablonlar:**
   - to'y va tug'ilgan kun taklifnomalari;
   - Navro'z, Ramazon hayiti, Qurbon hayiti, Yangi yil tabriklari;
   - Uzum Market, Wildberries va Ozon sotuvchilari uchun mahsulot videolari;
   - ko'chmas mulk (yangi uylar) reklamasi;
   - o'quv markazlari qabul e'lonlari;
   - restoran menyusi va aksiyalari;
   - **Telegram kanal postidan Reels/Shorts videosi** (kanal adminlari uchun juda kuchli taklif).
6. **Milliy dizayn.** O'zbek naqshlari (ornament) bilan tayyor mavzular (theme) hech kimda yo'q.

## 1.7. Maqsadli auditoriya (ICP)

| Segment | Og'riq | Qaysi platforma | To'lash ehtimoli |
|---|---|---|---|
| SMM menejer va agentliklar | Har kuni ko'p kontent kerak, dizayner qimmat va sekin | A + B | Yuqori (ishi shunga bog'liq) |
| Kichik biznes (restoran, do'kon, klinika, o'quv markazi) | Reklama video qilishni bilmaydi, agentlik qimmat | B (shablonlar) | O'rta |
| Marketplace sotuvchilari | Har bir mahsulot kartochkasiga video kerak | B (mahsulot shablonlari) + A (rasmdan video) | Yuqori (ommaviy ehtiyoj) |
| Kontent yaratuvchilar (Reels/TikTok) | Viral effektlar, B-roll | A (presetlar) | O'rta |
| Telegram kanal adminlari | Post ko'p, video yo'q | B (post → video, bot orqali) | O'rta |
| Keyinroq: xalqaro foydalanuvchilar | Arzon va tez motion video | B (ingliz tilida) | Yuqori ARPU |

## 1.8. Pozitsiyalash

- **Studio:** "O'zbek tilida yozing: kino sifatidagi AI video. Barcha eng yaxshi modellar bitta joyda, so'mda to'lanadi."
- **Motion:** "Bir gap yozing, 2 daqiqada tayyor reklama video oling. Dizayner ham, montaj ham kerak emas."
- **Umumiy brend va'dasi:** "O'zbekiston va MDH uchun AI video studiya."

## 1.9. Xulosa: qaysi biri qiyinroq, qaysi biri foydaliroq

| Mezon | Studio (A) | Motion (B) |
|---|---|---|
| Talab | Juda yuqori | Yuqori (B2B ayniqsa) |
| Raqobat | Juda kuchli | O'rtacha |
| Texnik "moat" | Past (API wrapper) | **Yuqori** (komponentlar kutubxonasi + direktor + sifat nazorati) |
| MVPga vaqt | Qisqa (3 hafta) | O'rta (5 hafta) |
| Marja | ~55-60% | ~75-85% |
| Lokal ustunlik | Til, to'lov, presetlar | Til, ovoz, mahalliy shablonlar, Telegram |

**Strategiya:** Studio tez trafik, pul oqimi va to'lov tajribasini beradi. Motion esa uzoq muddatli himoyalangan va yuqori marjali biznesga aylanadi. Ikkalasi bitta kredit hamyonini ishlatadi va bir-birini to'ldiradi: Studio kliplari Motion'da B-roll bo'ladi, Motion foydalanuvchisi esa Studio'ga o'tadi.

## Manbalar

- Higgsfield narxlari: [Krea](https://www.krea.ai/blog/higgsfield-pricing-explained-2026-unlimited-credits-and-real-monthly-costs), [HeyGen blog](https://www.heygen.com/blog/higgsfield-pricing), [Hackceleration review](https://hackceleration.com/labs/review/higgsfield), [costbench](https://costbench.com/software/ai-video-generators/higgsfield/)
- Google Flow: [Wikipedia: Veo](https://en.wikipedia.org/wiki/Veo_(text-to-video_model)), [Simpliaxis](https://www.simpliaxis.com/resources/what-is-google-flow), [Bonega.ai](https://bonega.ai/en/blog/google-flow-march-2026-unified-ai-video-workspace), [Vidau](https://www.vidau.ai/google-flow-ai-update-2026-complete-guide/)
- Claude Motion: [Anthropic yordam markazi](https://support.claude.com/en/articles/17454997), [Runway e'loni](https://runway.com/news/company-news/runway-claude-motion), [eesel.ai](https://www.eesel.ai/blog/claude-motion)
- Sora yopilishi: [Wikipedia: Sora](https://en.wikipedia.org/wiki/Sora_(text-to-video_model)), [techjacksolutions](https://techjacksolutions.com/ai-brief/openai-videos-api-sora-2-deprecated-september-2026/), [invideo](https://invideo.io/blog/sora-ai-video-generator/)
- HyperFrames: [GitHub: heygen-com/hyperframes](https://github.com/heygen-com/hyperframes)
- Motion-grafika vositalari: [Vidu](https://www.vidu.com/tools/ai-motion-graphics-generator), [VideoGen](https://videogen.io/ai-motion-graphics-generator), [Pexo](https://pexo.ai/blog/programmatic-vs-ai-generated-video-with-claude-code-remotion-hyperframes-and-pexo-compared-6108)
