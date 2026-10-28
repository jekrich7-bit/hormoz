# 10 — Platforma skeleti va texnik spetsifikatsiya

Bu hujjat Claude Code uchun "qurilish chizmasi". Unda papkalar tuzilmasi, marshrutlar, ma'lumotlar modeli, fon ishlari va texnik talablar bor.

---

## 1. Texnologiyalar va versiyalar

| Soha | Tanlov | Izoh |
|---|---|---|
| Framework | **Next.js 16.4.x** (App Router), React 19 | ✅ 2026-yil oktabr holatida eng so'nggi barqaror versiya. Xavfsizlik yangilanishlari sababli 16.4.1 yoki undan yangi versiya ishlatilsin |
| Til | TypeScript (`strict`) | |
| Stil | Tailwind CSS + shadcn/ui, Lucide ikonkalar | Dizayn tokenlari CSS o'zgaruvchilarida (08-hujjat) |
| Baza | PostgreSQL 16+ va Drizzle ORM | Migratsiyalar `drizzle-kit` orqali |
| Navbat | **pg-boss** | Fon ishlari uchun; Postgres'da ishlaydi, Redis kerak emas |
| Auth | **Better Auth** (`organization`, `admin`, `twoFactor` plaginlari) + Telegram kirish | Telegram Login Widget va Mini App ma'lumotlari serverda HMAC orqali tekshiriladi |
| Validatsiya | zod (umumiy sxemalar: klient, server va AI chiqishi uchun) | |
| Formalar va jadvallar | react-hook-form, TanStack Table | |
| Grafiklar | Recharts | |
| i18n | next-intl (`uz-Latn`, `ru`) | |
| Fayllar | S3-mos ombor (Supabase Storage, Cloudflare R2 yoki MinIO) + imzolangan havolalar | |
| AI | `@anthropic-ai/sdk`, model `claude-opus-5-5` | Structured outputs, prompt caching, Batch |
| Nutq | STT provayderi (sinov natijasiga qarab), Azure Pronunciation Assessment, TTS (Azure yoki ElevenLabs) | 03-hujjat |
| Takrorlash | ts-fsrs | |
| Monitoring | Sentry (xatolar), PostHog (mahsulot analitikasi) | |
| Testlar | Vitest (unit), Playwright (E2E) | |
| Yetkazish | Docker (ilova + worker), GitHub Actions | 03-hujjat, hosting variantlari |

Kutubxona nomlari va API'lar o'rnatish paytida rasmiy hujjatlardan (Context7 MCP orqali) tekshiriladi.

---

## 2. Papkalar tuzilmasi

```
hormoz/
├── app/
│   ├── [locale]/
│   │   ├── (public)/                 # bosh sahifa, narxlar, markazlar, aniqlik, qo'llanmalar, huquqiy
│   │   ├── (auth)/                   # kirish, onboarding
│   │   ├── app/                      # talaba ilovasi (07-hujjat, 3-bo'lim)
│   │   ├── imtihon/[attemptId]/      # imtihon rejimi (alohida layout)
│   │   ├── markaz/                   # B2B portal
│   │   └── admin/                    # admin panel (09-hujjat)
│   └── api/
│       ├── auth/[...all]/            # Better Auth
│       ├── webhooks/payme/           # Payme Merchant API (JSON-RPC)
│       ├── webhooks/click/           # Click: prepare / complete
│       ├── webhooks/uzum/            # Uzum: check / create / confirm / reverse / status
│       ├── webhooks/telegram/        # bot
│       ├── uploads/                  # imzolangan yuklash havolalari
│       └── health/
├── components/
│   ├── ui/                           # shadcn
│   ├── exam/                         # ExamTimer, QuestionNavigator, SplitPane, Recorder ...
│   ├── feedback/                     # AnnotatedEssay, CriterionBar, TranscriptPlayer
│   ├── admin/                        # DataTable, TestBuilder, ReviewWorkspace
│   └── charts/
├── lib/
│   ├── db/                           # Drizzle sxema va so'rovlar
│   ├── auth/                         # sessiya, Telegram tekshiruvi
│   ├── permissions/                  # can(user, action, resource)
│   ├── exam/                         # urinish holatlari, taymer, avtomatik saqlash
│   ├── scoring/                      # xom ball → band, yaxlitlash, Writing og'irliklari (sof funksiyalar)
│   ├── normalize/                    # javoblarni solishtirish qoidalari
│   ├── cat/                          # Rasch hisoblari, keyingi savolni tanlash
│   ├── srs/                          # ts-fsrs ustidagi qatlam
│   ├── ai/                           # Claude mijozi, prompt reestri, JSON sxemalar
│   ├── speech/                       # STT, pronunciation, TTS adapterlari
│   ├── payments/                     # Payme, Click, Uzum adapterlari
│   ├── quotas/                       # tarif limitlari
│   ├── audit/                        # audit jurnali
│   └── i18n/
├── worker/
│   ├── index.ts                      # pg-boss ishchisi
│   └── jobs/                         # score-writing, score-speaking, tts, batch-poll, regroup, reports
├── prompts/                          # versiyalangan promptlar (reestr bilan sinxronlanadi)
├── evals/                            # kalibratsiya skriptlari, QWK/MAE
├── drizzle/                          # migratsiyalar
├── messages/                         # uz.json, ru.json
├── tests/
│   ├── unit/
│   └── e2e/
├── docs/
├── CLAUDE.md
├── docker-compose.yml                # lokal Postgres + MinIO
└── .github/workflows/                # CI
```

---

## 3. Marshrutlar va himoya

| Marshrut | Kim kira oladi | Himoya |
|---|---|---|
| `/(public)/*` | Hamma | — |
| `/app/*` | Talaba | Sessiya bo'lishi shart |
| `/imtihon/[id]` | Urinish egasi | Sessiya + urinish egasi + urinish holati `in_progress` |
| `/markaz/*` | Markaz admini, o'qituvchi | Sessiya + tashkilot a'zoligi + rol |
| `/admin/*` | Admin rollari | Sessiya + rol + **2FA** |
| `/api/webhooks/*` | Provayderlar | Imzo yoki login-parol tekshiruvi, IP ro'yxati (provayder bersa) |

Har bir server action boshida `can(user, action, resource)` chaqiriladi. Middleware faqat birinchi filtr.

---

## 4. Ma'lumotlar modeli

### 4.1. Foydalanuvchilar va tashkilotlar
- `users`: id, ism, telegram_id, telefon, til, rol(lar), yaratilgan sana
- `profiles`: user_id, maqsad band, imtihon sanasi, modul (academic/gt), ichki daraja (L0–L6), haftalik soat
- `organizations`: id, nomi, turi (markaz), logotip, holati
- `memberships`: user_id, org_id, rol (org_admin, teacher, student)
- `sessions`, `accounts`, `two_factor`: Better Auth jadvallari

### 4.2. Kontent
- `tests`: id, nomi, modul, holat, versiya, blueprint_id, muallif
- `test_sections`: test_id, ko'nikma (L/R/W/S), tartib, vaqt (soniya)
- `parts`: section_id, tartib, sarlavha, stimulus (JSON: matn yoki skript), media_id
- `question_groups`: part_id, turi (enum), ko'rsatma, shared (JSON), boshlanish va tugash raqami
- `questions`: group_id, raqam, prompt, variantlar (JSON), javob kaliti (JSON), qabul qilinadigan variantlar, so'z chegarasi, dalil (JSON: joylashuv), izoh (uz, en), CEFR, teglar
- `media`: id, turi, url, davomiyligi, manba (yuklangan/TTS), TTS parametrlari
- `blueprints`: bo'lim talablari (JSON)
- `content_versions`: obyekt turi, obyekt id, versiya, snapshot (JSON), muallif
- `content_reviews`: obyekt, holat o'tishi, tekshiruvchi, izoh, tekshiruv ro'yxati (JSON)
- `lessons`, `modules`, `tracks`, `vocab_decks`, `vocab_items`

### 4.3. Urinishlar va baholash
- `attempts`: id, user_id, test_id (yoki placement), turi (full/section/placement/practice), holat, boshlangan va tugagan vaqt, `deadline_at`, mijoz qurilmasi
- `responses`: attempt_id, question_id, javob, to'g'rimi, sarflangan vaqt, flag, yangilangan vaqt
- `writing_submissions`: attempt_id, task (1/2), matn, so'zlar soni
- `speaking_submissions`: attempt_id, part, savol, audio_media_id, davomiyligi
- `transcripts`: speaking_submission_id, matn, so'zlar (JSON: so'z, boshlanish, tugash), provayder
- `ai_scores`: submission turi va id, `model_id`, `prompt_version`, `rubric_version`, mezonlar (JSON), umumiy band, ishonch, feedback (JSON), token va xarajat, yaratilgan vaqt
- `human_scores`: submission, mutaxassis, mezonlar, umumiy band, izoh, ko'r rejimdami (blind), sarflangan vaqt
- `section_results`: attempt_id, ko'nikma, xom ball, band, manba (auto/ai/human)
- `placement_results`: attempt_id, θ, SE, CEFR, ko'nikmalar bo'yicha band va oraliq, tavsiya etilgan yo'l
- `review_tasks`: submission, sabab (paid/low_confidence/sample/appeal), biriktirilgan mutaxassis, muddat, holat
- `calibration_sets`, `calibration_items`, `eval_runs`: model, prompt, metrikalar (JSON)
- `real_results`: user_id, TRF fayli, tekshirildimi, band'lar, imtihon sanasi, rozilik

### 4.4. O'qitish
- `cohorts`: daraja, o'qituvchi, jadval (JSON), hajm, boshlanish sanasi, org_id
- `cohort_members`: cohort_id, user_id, qo'shilgan va chiqqan sana, sabab
- `srs_cards`: user_id, manba (vocab/xato), kontent (JSON), FSRS holati
- `srs_reviews`: card_id, baho, vaqt
- `daily_plans`: user_id, sana, vazifalar (JSON), bajarilganlari

### 4.5. Savdo
- `plans`: kod, narx, muddat, limitlar (JSON), versiya, faolmi
- `subscriptions`: user_id yoki org_id, plan_id, holat, davr boshi va oxiri
- `usage_counters`: user_id, davr, mock, writing, speaking soni
- `payments`: provayder, provayder tranzaksiya id, summa, holat, asl ma'lumot (JSON), user_id, plan_id
- `refunds`, `promo_codes`, `promo_redemptions`, `invoices`, `referrals`

### 4.6. Tizim
- `audit_logs`: aktor, amal, obyekt, oldin va keyin (JSON), IP, vaqt
- `feature_flags`, `support_tickets`, `notifications`, `broadcasts`, `events` (Mock Day)

### 4.7. Savollar guruhi namunasi (JSON)

```json
{
  "type": "matching_headings",
  "instructions": "Choose the correct heading for each paragraph from the list of headings below.",
  "shared": {
    "headings": [
      { "id": "i", "text": "The origins of trade routes" },
      { "id": "ii", "text": "Why merchants avoided the sea" }
    ]
  },
  "questions": [
    {
      "number": 14,
      "target": "A",
      "answer": ["ii"],
      "evidence": { "paragraph": "A", "start": 120, "end": 214 },
      "explanation": { "uz": "...", "en": "..." }
    }
  ]
}
```

Har bir savol turi uchun alohida zod sxema bo'ladi. Admin konstruktori, talaba ekrani va tekshiruvchi bitta sxemadan foydalanadi.

---

## 5. Imtihon dvigateli qoidalari

1. **Holatlar:** `created → in_progress → (section_submitted)* → submitted → scoring → scored`. Agar vaqt tugasa, urinish `expired` holatiga o'tadi va server uni avtomatik topshiradi.
2. **Taymer serverda boshqariladi:** har bir bo'lim uchun `deadline_at` serverda saqlanadi. Klient faqat ko'rsatadi. Muddatdan keyin kelgan javoblar (kichik tarmoq zaxirasidan tashqari) qabul qilinmaydi.
3. **Avtomatik saqlash:** o'zgarishlar 5–10 soniyada to'plab yuboriladi; oflaynda `IndexedDB`'ga yoziladi va qayta ulanganda yuboriladi.
4. **Listening:** audio oldindan to'liq yuklanadi; uni qaytarib yoki to'xtatib bo'lmaydi; brauzer yopilib qayta ochilsa, audio vaqtga mos joydan davom etadi.
5. **Writing:** `spellcheck="false"`, `autocorrect="off"`, `autocapitalize="off"`; tashqaridan matn joylashtirish qayd qilinadi (halollik signali).
6. **Halollik signallari:** tabdan chiqishlar soni, oynadan uzoq vaqt chiqish, joylashtirilgan matn hajmi. Bular faqat qayd qilinadi va hisobotda ko'rsatiladi; avtomatik jazo berilmaydi.

## 6. Javoblarni solishtirish (`lib/normalize`)

- Katta-kichik harf, boshi va oxiridagi bo'shliqlar, ortiqcha tinish belgilari e'tiborga olinmaydi.
- Raqamlar: "3" va "three" teng (agar savol ruxsat bersa); sanalar va vaqtlar uchun qabul qilinadigan formatlar ro'yxati.
- Britaniya va Amerika imlosi (colour/color) — savolda ruxsat berilgan bo'lsa.
- So'z chegarasi ("NO MORE THAN TWO WORDS") oshib ketsa, javob noto'g'ri hisoblanadi.
- Har bir savolning qabul qilinadigan variantlari admin'da kiritiladi.

## 7. Fon ishlari (pg-boss)

| Ish | Qachon ishga tushadi | Nima qiladi |
|---|---|---|
| `score-writing` | Writing topshirilganda | Claude → JSON → `ai_scores`; kerak bo'lsa `review_tasks` yaratadi |
| `score-speaking` | Speaking qismi yuklanganda | STT → xususiyatlar → pronunciation → Claude |
| `finalize-attempt` | Barcha bo'limlar baholanganda | Umumiy band, hisobot va Telegram bildirishnomasi |
| `batch-submit` / `batch-poll` | Har 10 daqiqada | Shoshilinch bo'lmagan tekshiruvlarni Batch API orqali yuborish va natijasini olish |
| `tts-generate` | Admin so'raganda | Skript → audio fayl |
| `daily-plan` | Har kuni 05:00 | Har bir faol talaba uchun bugungi reja |
| `srs-reminders` | Har kuni | Telegram eslatmalari |
| `regroup` | Har 4 haftada | Qayta guruhlash takliflari |
| `item-stats` | Har kecha | Savollar statistikasi va bayroqlar |
| `reconcile-payments` | Har kecha | Provayder hisobotlari bilan solishtirish |
| `cost-report` | Har kuni | AI xarajati va limitdan oshish ogohlantirishi |
| `cleanup-audio` | Har kuni | Saqlash muddati tugagan audio fayllarni o'chirish (siyosatga ko'ra) |

## 8. Xavfsizlik va maxfiylik

- Kalitlar faqat muhit o'zgaruvchilarida; `.env` repoga qo'shilmaydi; Claude Code hook'i `.env` faylini tahrirlashni bloklaydi.
- Rate limit: kirish, AI tekshiruvlari va webhook'lar uchun.
- Fayllar faqat imzolangan va qisqa muddatli havolalar orqali yuklanadi va ko'riladi.
- Audio uchun alohida rozilik so'raladi; saqlash muddati siyosati (masalan, 180 kun), talaba o'z yozuvlarini o'chira oladi.
- Admin: 2FA, audit, eksportni cheklash.
- Telegram ma'lumotlari va to'lov webhook'lari imzo bilan tekshiriladi; to'lov webhook'larini qayta yuborish xavfsiz (idempotent).
- Bog'liqliklar haftalik tekshiriladi; Next.js xavfsizlik yangilanishlari kuzatib boriladi.

## 9. Muhitlar va CI/CD

| Muhit | Tavsifi |
|---|---|
| Lokal | `docker compose up` (Postgres + MinIO), `npm run dev`, `npm run worker` |
| Staging | Production'ning nusxasi, sinov to'lov kalitlari, haqiqiy bo'lmagan ma'lumotlar |
| Production | 03-hujjat: A yoki B varianti |

**CI (GitHub Actions), har bir PR'da:** lint → typecheck → unit testlar → build → Playwright E2E (asosiy oqimlar). `prompts/` yoki `lib/ai/` o'zgarsa, qo'shimcha ravishda `eval:scoring` ishga tushadi.

## 10. Analitika hodisalari (PostHog)

`signup`, `placement_started`, `placement_completed`, `mock_started`, `section_submitted`, `mock_completed`, `feedback_viewed`, `rewrite_submitted`, `srs_session_completed`, `paywall_viewed`, `checkout_started`, `payment_succeeded`, `subscription_canceled`, `human_review_requested`, `appeal_submitted`, `real_result_uploaded`.

## 11. Ishlash talablari (maqsad)

- Imtihon sahifasi o'rtacha mobil internetda 2,5 soniyadan tezroq ochiladi.
- Javobni saqlash 300ms dan tezroq (95% hollarda).
- Writing feedback 2 daqiqadan tezroq keladi (real vaqt rejimida).
- Bir vaqtda 300 kishi Mock Day'da qatnashganda ham ishlaydi (yuklama testi bilan tekshiriladi).

## 12. Boshlang'ich ma'lumotlar (seed)

- 1 ta namunaviy to'liq test (har bir savol turi bilan)
- 3 ta tarif
- Har bir rol uchun bittadan sinov foydalanuvchisi
- 50 ta placement savoli
- 20 ta namunaviy insho (kalibratsiya oqimini sinash uchun)
