# 05 — Yo'l xaritasi: qadamma-qadam

Har bir bosqichda "Tayyor" mezoni bor. U bajarilmaguncha keyingi bosqichga o'tilmaydi.
Muddatlar (🧮) bir kishi Claude Code bilan, haftasiga ~25–30 soat ishlaydi degan farazga asoslangan.

---

## 0-bosqich. Qarorlar va tayyorgarlik (1-hafta)

**Siz qabul qiladigan qarorlar** (oxiridagi "Ochiq savollar"ga qarang):
- [ ] Academic, General Training yoki ikkalasi (tavsiya: avval Academic)
- [ ] Brend nomi ("IELTS" so'zisiz) va domen
- [ ] Hosting varianti: A (tez MVP) yoki B (O'zbekistonda) — yurist bilan
- [ ] YaTT yoki MChJ ochish (to'lov provayderlari uchun shart)

**Texnik tayyorgarlik:**
- [ ] Anthropic API kaliti, xarajat limiti bilan
- [ ] STT va Pronunciation provayderlarida sinov akkauntlari
- [ ] Telegram bot (BotFather)
- [ ] Payme, Click va Uzum bilan merchant arizalari (bir necha hafta ketishi mumkin, shuning uchun hozirdan)
- [ ] 2 nafar sertifikatli mutaxassis-baholovchi bilan kelishuv (kalibratsiya uchun)
- [ ] **STT tanlash testi** (03-hujjat, 6-bo'lim)

**Tayyor:** barcha akkauntlar ochilgan, STT tanlangan, `CLAUDE.md` repoga qo'shilgan.

---

## 1-bosqich. Listening va Reading mock dvigateli (2–4-haftalar)

| # | Vazifa (Claude Code'ga Plan mode'da beriladi) | Qabul mezoni |
|---|---|---|
| 1.1 | Next.js loyiha + Drizzle + PostgreSQL + i18n (uz/ru) skeleti | `npm run dev`, lint va typecheck toza |
| 1.2 | Telegram Login | Kirish va chiqish ishlaydi, profil yaratiladi |
| 1.3 | Ma'lumotlar modeli: tests, sections, questions, attempts, responses | Migratsiya + seed ma'lumotlar |
| 1.4 | Imtihon UI: taymer, savollar paneli, flag, highlight, eslatma, ikkiga bo'lingan Reading ekrani, Listening pleyeri (bir marta ijro, imtihondagidek) | Playwright E2E: to'liq L+R urinishi boshidan oxirigacha |
| 1.5 | Avtomatik saqlash va internet uzilishidan keyin tiklash | Sahifa yangilansa ham javoblar saqlanib qoladi |
| 1.6 | Avtomatik tekshirish + xom ball → band + natija sahifasi | `lib/scoring` unit testlari (yaxlitlash, chegaralar) |
| 1.7 | Admin panel: test va savollarni kiritish | Muharrir kodsiz test qo'sha oladi |
| 1.8 | 2 ta original L+R test (TTS audio bilan) | `content-reviewer` subagenti va inson tekshiruvidan o'tgan |

**Tayyor:** 10 nafar tanish odam 2 ta testni xatosiz topshirib chiqadi.

---

## 2-bosqich. Writing va Speaking AI baholash (5–8-haftalar)

| # | Vazifa | Qabul mezoni |
|---|---|---|
| 2.1 | Kalibratsiya to'plamini yig'ish: 100+ insho, 2 nafar ekspert bahosi bilan | Bazada saqlangan, ekspertlar o'rtasidagi moslik hisoblangan |
| 2.2 | `eval:scoring` skripti: QWK, MAE, ±0.5 moslik | Bitta buyruq bilan hisobot chiqaradi |
| 2.3 | Writing baholash: navbat + Claude Opus 5.5 + JSON sxema + prompt caching | Kalibratsiyada MAE ≤ 0.5 (ichki maqsad) |
| 2.4 | Writing feedback UI: mezonlar, xatolar matnda belgilangan, "qayta yozish" | O'quvchi qayta yozadi va farqni ko'radi |
| 2.5 | Speaking: imtihon oluvchi ovozi (TTS), `MediaRecorder` bilan yozish, yuklash | Telefon va kompyuter brauzerlarida ishlaydi |
| 2.6 | Speaking konveyeri: STT → xususiyatlar → Pronunciation → Claude | 50 ta speaking namunasida inson bilan solishtirilgan |
| 2.7 | Inson navbati: past ishonchli ishlar ekspertga | Ekspert paneli |
| 2.8 | Placement v1 (qat'iy shaklda, CEFR teglari bilan, 30–40 savol + mini Writing/Speaking) | Natija sahifasida daraja, band ± oraliq va yo'l tavsiyasi |

**Tayyor:** to'liq mock (L+R+W+S) ishlaydi; AI aniqligi o'lchangan va yozib qo'yilgan.

---

## 3-bosqich. To'lov va yopiq beta (9–12-haftalar)

| # | Vazifa | Qabul mezoni |
|---|---|---|
| 3.1 | Payme + Click (keyin Uzum) integratsiyasi, webhook'lar | Sandbox'da barcha holatlar: muvaffaqiyat, bekor qilish, qaytarish |
| 3.2 | Tariflar va oylik limitlar (`usage_quotas`) | Limit tugaganda to'g'ri xabar va yangilash taklifi |
| 3.3 | O'quvchi paneli: progress grafigi, zaif ko'nikmalar, keyingi qadam | |
| 3.4 | FSRS: so'zlar va xatolar jurnali kartochkalari | Kunlik takrorlash ishlaydi |
| 3.5 | Telegram bot: eslatmalar, natijalar, "Mock Day" | |
| 3.6 | Disclaimer, maxfiylik siyosati, foydalanish shartlari, rozilik (audio uchun) | Yurist ko'rib chiqqan |
| 3.7 | Sentry + PostHog + AI xarajatlari monitoringi | Har kuni xarajat hisoboti |
| 3.8 | **Yopiq beta: 100 nafar foydalanuvchi** | Haqiqiy to'lovlar, KPI'lar o'lchangan |

**Tayyor:** kamida 20 ta pullik mijoz, D30 saqlanish o'lchangan, birinchi haqiqiy IELTS natijalari (TRF) yig'ila boshlagan.

---

## 4-bosqich. O'qitish va guruhlash (4–6-oylar)

- 4.1 Band 5, 6 va 7 yo'llari uchun mikro-darslar (02-hujjat, 5-bo'lim)
- 4.2 Kogortalar, guruhlash algoritmi va har 4 haftada qayta guruhlash
- 4.3 Placement v2: savollar kalibrlangach (har biriga 100–250 javob) CAT'ga o'tish
- 4.4 B2B o'quv markazi paneli: o'quvchilar, guruhlar, hisobotlar
- 4.5 "7.0 Sprint" taklifi; aniqlik kafolati faqat MAE ≤ 0.5 bo'lsa yoqiladi
- 4.6 Saytda ochiq aniqlik hisoboti

**Tayyor:** bazaviy ssenariyga qarab harakat (04-hujjat, 6-bo'lim) va zararsizlik nuqtasiga yaqinlashish.

---

## 5-bosqich. Kengayish (6–12-oylar)

- Band 8 yo'li va premium 1:1 murabbiylik
- Mutaxassis-baholovchilar marketplace'i
- CEFR Multilevel mock — ikkinchi mahsulot liniyasi (01-hujjat, A4)
- General Training
- PWA (alohida mobil ilova hozircha shart emas)
- Speaking Part 3 uchun dinamik qo'shimcha savollar

---

## Ochiq savollar (sizning qaroringiz kerak)

1. **Asosiy auditoriya kim:** maktab o'quvchilari va abituriyentlar (16–19), talabalar yoki ishlaydiganlar?
2. **Academic yoki General Training:** qaysi biri birinchi?
3. **Byudjet:** boshlang'ich ~44 mln so'm va oyiga ~25 mln so'm doimiy xarajat (04-hujjat) sizga to'g'ri keladimi?
4. **Jamoa:** kontent muharriri va mutaxassis-baholovchini topa olasizmi, yoki men ularga talablar ro'yxatini tuzib beraymi?
5. **Hosting:** yurist bilan shaxsiy ma'lumotlarni lokalizatsiya qilish masalasini qachon ko'rib chiqasiz?
6. **Brend nomi:** "Hormoz" ishchi nom bo'lib qoladimi?
