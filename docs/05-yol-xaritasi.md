# 05 — Yo'l xaritasi: sprintlar bo'yicha

- **Sprint** — 2 hafta (Sprint 0 — 1 hafta). Jami ~25 hafta, ya'ni taxminan 6 oy.
- 🧮 Faraz: bir kishi Claude Code bilan haftasiga ~25–30 soat ishlaydi; kontent va tashkiliy ishlar parallel ravishda boradi.
- Har bir sprint oxirida **namoyish** bo'ladi va "Tayyor" mezoni tekshiriladi. Mezon bajarilmasa, keyingi sprintga o'tilmaydi.
- Bog'liq hujjatlar: bo'limlar 07, dizayn 08, admin 09, skelet 10.

---

## Asosiy bosqichlar (milestone)

| # | Hafta | Natija |
|---|---|---|
| M1 | 5 | Ichki L/R mock: admin'da yaratilgan test talaba ekranida to'liq ishlaydi |
| M2 | 9 | To'liq mock (L+R+W+S) AI bahosi va o'lchangan aniqlik bilan |
| M3 | 13 | To'lov bilan yopiq beta: 100 foydalanuvchi |
| M4 | 21 | O'qitish, guruhlar va B2B portal |
| M5 | 25 | Ochiq ishga tushirish (O'zbekistondagi hostingda) |

---

## Sprint 0 (1-hafta) — Poydevor

| Yo'nalish | Vazifalar |
|---|---|
| Kod | Next.js skeleti (10-hujjat, 2-bo'lim), Drizzle + lokal Postgres, i18n (uz/ru), dizayn tokenlari, CI (lint, typecheck, test) |
| Auth | Better Auth + Telegram kirish, rollar |
| Admin | Admin qobig'i: sidebar, ⌘K, ruxsatlar (`can()`), 2FA, audit jurnali |
| Claude Code | `CLAUDE.md`, Playwright va Context7 MCP, hook'lar (lint/typecheck, `.env` himoyasi) |
| Siz (kodsiz) | Brend nomi, YaTT yoki MChJ ochish, to'lov provayderlariga ariza, mutaxassislar bilan kelishuv, STT tanlash testi uchun 20 ta yozuv yig'ish |

**Tayyor:** kirish ishlaydi, admin faqat 2FA bilan ochiladi, CI yashil.

## Sprint 1 (2–3-haftalar) — Reading: konstruktor + imtihon ekrani

| Yo'nalish | Vazifalar |
|---|---|
| Admin | Test konstruktori (Reading), Reading uchun barcha savol turlari, jonli ko'rinish, savollar banki, media kutubxona |
| Talaba | Imtihon rejimi: taymer, SplitPane, belgilash va eslatma, flag, savollar paneli, sozlamalar (shrift, kontrast), avtomatik saqlash |
| Mantiq | `lib/scoring` va `lib/normalize` unit testlar bilan |
| Kontent | 1-test: Reading qismi |

**Tayyor:** Playwright testi admin'da yaratilgan Reading testini boshidan oxirigacha topshiradi va to'g'ri band chiqadi.

## Sprint 2 (4–5-haftalar) — Listening + natijalar + bosh sahifa → **M1**

| Yo'nalish | Vazifalar |
|---|---|
| Admin | Listening muharriri, TTS generator, blueprint tekshiruvi, kontent workflow (qoralama → nashr) |
| Talaba | Listening ekrani (bir marta ijro, oxirida 2 daqiqa tekshirish), L/R natija hisoboti (dalil bilan) |
| Ommaviy | Bosh sahifa, narxlar (hozircha statik), huquqiy sahifalar, disclaimer |
| Kontent | 2 ta to'liq L+R test |

**Tayyor:** 10 nafar tanish odam 2 ta L+R testni xatosiz topshiradi.

## Sprint 3 (6–7-haftalar) — Writing AI

| Yo'nalish | Vazifalar |
|---|---|
| Admin | Prompt reestri, AI navbati, kalibratsiya to'plamlari, eval paneli, AI kontent studiyasi (qoralama yaratish) |
| Talaba | Writing ekrani (so'z hisoblagichi, imlo tekshiruvi o'chiq), Writing feedback (AnnotatedEssay), qayta yozish |
| Backend | pg-boss worker, `score-writing`, structured output, prompt caching, model/prompt versiyasini saqlash |
| Eval | `npm run eval:scoring`: QWK, MAE, ±0.5 moslik |
| Siz | Kalibratsiya uchun 100+ insho va har biriga 2 ta mutaxassis bahosi |

**Tayyor:** kalibratsiya to'plamida o'lchangan aniqlik hisoboti tayyor; feedback 2 daqiqadan tezroq keladi.

## Sprint 4 (8–9-haftalar) — Speaking + inson tekshiruvi → **M2**

| Yo'nalish | Vazifalar |
|---|---|
| Talaba | Speaking ekrani (TTS imtihon oluvchi, Part 2 tayyorgarlik, Recorder), Speaking hisoboti (TranscriptPlayer) |
| Backend | `score-speaking`: STT → fluency xususiyatlari → Azure Pronunciation → Claude |
| Admin | Inson tekshiruvi ish joyi (ko'r rejim), navbat qoidalari, mutaxassislar statistikasi |
| Kontent | 4 ta to'liq test (L+R+W+S) |

**Tayyor:** to'liq mock ishlaydi; 50 ta speaking namunasida AI va inson bahosi solishtirilgan.

## Sprint 5 (10–11-haftalar) — Placement + bosh panel

| Yo'nalish | Vazifalar |
|---|---|
| Talaba | Placement v1 (qat'iy shakl), natija sahifasi (radar, yo'l, PDF), "Bugungi reja" bosh paneli, natijalar tarixi va progress |
| Admin | Foydalanuvchilar moduli, bosh panel KPI'lari |
| Integratsiya | Telegram bot: natija tayyor, eslatmalar |
| Kontent | 300 ta placement savoli (CEFR teglari bilan) |

**Tayyor:** placement'ni boshlash, natijani olish va yo'lni tanlash oqimi to'liq ishlaydi.

## Sprint 6 (12–13-haftalar) — To'lov va yopiq beta → **M3**

| Yo'nalish | Vazifalar |
|---|---|
| Backend | Payme va Click (keyin Uzum), webhook'lar, idempotentlik, tarif limitlari |
| Talaba | Obuna sahifasi, paywall, promo kod |
| Admin | Tariflar, promo kodlar, tranzaksiyalar, qaytarishlar, murojaatlar |
| Operatsiya | Sentry, PostHog, AI xarajatlari hisoboti; maxfiylik siyosati va oferta (yurist ko'rib chiqqan) |
| Beta | 100 nafar foydalanuvchi, haqiqiy to'lovlar |

**Tayyor:** kamida 20 ta pullik mijoz; KPI'lar (04-hujjat, 8-bo'lim) o'lchanmoqda.

## Sprint 7–8 (14–17-haftalar) — O'qitish

| Yo'nalish | Vazifalar |
|---|---|
| Talaba | Kurs ("Mening yo'lim"), darslar va darvoza testlari, mashqlar bo'limi, FSRS lug'at va xatolar jurnali |
| Admin | Darslar, modullar, yo'llar, lug'at to'plamlari (CSV import) |
| Kontent | L2, L3 va L4 yo'llari uchun birinchi modullar |

**Tayyor:** o'quvchi placement → yo'l → kunlik reja → takrorlash siklini to'liq bosib o'tadi.

## Sprint 9–10 (18–21-haftalar) — Guruhlar, B2B, marketing → **M4**

| Yo'nalish | Vazifalar |
|---|---|
| Talaba | Guruhim sahifasi, Mock Day va reyting |
| Markaz portali | O'quvchilar, guruhlar, topshiriqlar, hisobotlar, litsenziya |
| Admin | Kogortalar va avtomatik guruhlash, B2B markazlar, Telegram xabarnomalar, Mock Day, analitika |
| Biznes | Birinchi 2–3 ta markaz bilan pilot; "7.0 Sprint" taklifi |

**Tayyor:** kamida 2 ta markaz portaldan foydalanmoqda.

## Sprint 11–12 (22–25-haftalar) — Ishga tushirish → **M5**

| Yo'nalish | Vazifalar |
|---|---|
| Sifat | Savol statistikasi va bayroqlar, yuklama testi (Mock Day uchun 300 kishi), xavfsizlik tekshiruvi (`/security-review`) |
| Placement | Savollar yetarli javob yig'gan bo'lsa, IRT kalibratsiyasi; CAT faqat shundan keyin |
| Infratuzilma | O'zbekistondagi hostingga ko'chish (B varianti), zaxira nusxalar, to'lovlarni solishtirish |
| Ommaviy | Ochiq aniqlik hisoboti sahifasi, qo'llanmalar (SEO) |

**Tayyor:** ochiq ishga tushirish.

---

## Parallel yo'nalish: kodsiz ishlar (siz va jamoa)

| Hafta | Ish |
|---|---|
| 1 | Brend nomi, yuridik shaxs, to'lov provayderlariga arizalar, mutaxassislar bilan kelishuv |
| 1–4 | Kontent muharririni topish; kontent qo'llanmasini yozish (men qoralamasini tayyorlayman) |
| 3–8 | Kalibratsiya ma'lumotlari: insholar va speaking yozuvlari, ularning mutaxassis baholari |
| Har sprint | +2 ta original test (AI qoralama → muharrir → pilot) |
| 6–12 | Telegram kanalini o'stirish, haftalik bepul mashqlar, birinchi hamkor o'qituvchilar |
| 10–14 | Yurist: maxfiylik, oferta, shaxsiy ma'lumotlarni lokalizatsiya qilish |
| 14+ | O'quv markazlari bilan uchrashuvlar (B2B pilot) |

---

## Ochiq savollar (javobingiz kerak)

1. **MVP fokusi:** Academic + to'liq mock + Writing AI bilan boshlaymizmi? (Mening tavsiyam: ha.)
2. **Auditoriya:** maktab o'quvchilari va abituriyentlar, talabalar yoki ishlaydiganlar?
3. **Dizayn:** dengiz ko'ki + oltin rang va Inter shrifti ma'qulmi (08-hujjat)? Yoki boshqa yo'nalish kerakmi?
4. **Brend nomi:** "Hormoz" qoladimi?
5. **Admin:** 09-hujjatdagi rollar sizning jamoangizga mosmi? Kim qaysi rolda bo'ladi?
6. **Byudjet:** boshlang'ich ~44 mln so'm va oyiga ~25 mln so'm (04-hujjat) to'g'ri keladimi?
7. **Jamoa:** kontent muharriri va mutaxassis-baholovchini o'zingiz topasizmi, yoki ular uchun vakansiya va talablarni tayyorlab beraymi?
8. **Hosting:** A varianti (tez MVP) bilan boshlab, ishga tushirishdan oldin B variantiga o'tamizmi?
9. **Prototip:** kod yozishdan oldin 7 ta asosiy ekranning bosiladigan prototipini ko'rishni xohlaysizmi?
