# 03 — Texnik arxitektura va Claude Code sozlamasi

Ishonch belgilari: ✅ tekshirildi, 📚 taniqli manba (qayta tekshirilmadi), ⚠️ past ishonch, 🧮 faraz.
Narxlar 2026-yil oktabr holatiga. Byudjet tuzishdan oldin har bir provayderning rasmiy narx sahifasida tekshiring.

---

## 1. Eng kerakli vositalar (ustuvorlik tartibida)

| # | Qatlam | Tanlov | Nega aynan shu |
|---|---|---|---|
| 1 | Ishlab chiqish | **Claude Code** + `CLAUDE.md` + Plan mode + MCP | Butun loyihani bitta dasturchi (siz) qura oladi |
| 2 | Frontend + backend | **Next.js** (eng so'nggi barqaror versiya, App Router) + TypeScript + Tailwind CSS + shadcn/ui | Bitta kod bazasi, SSR, imtihon interfeysi uchun qulay |
| 3 | Ma'lumotlar bazasi | **PostgreSQL** + Drizzle ORM | Hamma joyda ishlaydi; Supabase'dan O'zbekistondagi serverga ko'chirish oson |
| 4 | Auth | **Telegram Login** + Google; keyinchalik SMS (mahalliy provayder) | O'zbekistonda deyarli hamma Telegram'da |
| 5 | Baholovchi AI | **Claude API — Claude Opus 5.5** (`claude-opus-5-5`), structured outputs, prompt caching, Batch API | Rubrika bo'yicha eng sifatli baholash; JSON sxema kafolati; kesh va batch narxni pasaytiradi |
| 6 | Nutqni matnga aylantirish (STT) | **Verbatim** STT, so'z vaqt belgilari bilan (nomzodlar: Deepgram Nova-3 `filler_words=true`, ElevenLabs Scribe, OpenAI gpt-4o-transcribe) | Fluency'ni o'lchash uchun "um", pauza va takrorlar saqlanishi shart. Tanlov sinov asosida qilinadi (6-bo'lim) |
| 7 | Talaffuzni baholash | **Azure Pronunciation Assessment** (yoki SpeechSuper/SpeechAce) | Pronunciation mezonini faqat akustik tahlil bilan baholash mumkin |
| 8 | Ovoz yaratish (TTS) | **Azure Neural** (arzon) yoki **ElevenLabs** (eng tabiiy, ko'p aksent) | Listening audio va imtihon oluvchi ovozi; bir marta yaratiladi va keshlanadi |
| 9 | To'lov | **Payme**, **Click**, **Uzum** merchant API | O'zbekiston bozori |
| 10 | Oraliqli takrorlash | **ts-fsrs** (FSRS algoritmi) | Ilmiy asoslangan zamonaviy algoritm |
| 11 | IRT / CAT | Ish vaqtida TypeScript'da Rasch hisoblari; kalibratsiya R `mirt` yoki Python'da (oflayn) | Placement aniqligi |
| 12 | Sifat | Vitest, **Playwright** (+ Playwright MCP), Sentry, PostHog | Imtihon jarayoni hech qachon buzilmasligi kerak |

---

## 2. Arxitektura sxemasi

```
 Foydalanuvchi (brauzer / Telegram Mini App)
        │
        ▼
 Next.js ilova (UI + API route'lar + server actions)
   ├── Imtihon dvigateli: taymer, javoblarni avtomatik saqlash, L/R avtomatik tekshirish
   ├── Placement (CAT): θ hisoblash, keyingi savolni tanlash
   ├── O'qitish: darslar, FSRS kartochkalari, xatolar jurnali
   ├── To'lov webhook'lari: Payme / Click / Uzum
   └── Navbat (job queue) ─────────────────────────────┐
        │                                              ▼
        ▼                                   Baholash ishchisi (worker)
 PostgreSQL + fayl ombori (audio)            ├── Writing → Claude (structured JSON)
                                             ├── Speaking → STT → xususiyatlar → Claude
                                             │              └→ Pronunciation (Azure)
                                             └── Natija + model/prompt versiyasi bazaga
```

**Asosiy qoidalar:**
1. Barcha API kalitlari faqat serverda saqlanadi.
2. AI baholash **navbat (queue)** orqali ishlaydi: so'rov to'xtab qolmaydi, qayta urinish (retry) va Batch rejimi oson qo'shiladi.
3. Har bir ball yonida `model_id`, `prompt_version` va `rubric_version` saqlanadi. Bu ilmiy shaffoflik va drift nazorati uchun kerak.
4. Javoblar har 5–10 soniyada avtomatik saqlanadi. Internet uzilsa ham test yo'qolmaydi.

### Hosting: huquqiy tanlov

| Variant | Qanday | Qachon |
|---|---|---|
| **A — tez MVP** | Vercel + Supabase (yaqin xorijiy region) | Faqat yopiq beta uchun, aniq rozilik bilan va yurist "mumkin" desa |
| **B — talabga mos** | Docker: Next.js (standalone) + PostgreSQL + S3-mos ombor (MinIO), O'zbekistondagi bulut yoki VPS'da | Ochiq ishga tushirishdan oldin. 📚 Shaxsiy ma'lumotlarni lokalizatsiya qilish talabi sababli (01-hujjat, C bo'limi) |

Drizzle + oddiy PostgreSQL tanlangani uchun A dan B ga ko'chish bir kunlik ish bo'ladi.

---

## 3. Ma'lumotlar modeli (asosiy jadvallar)

| Jadval | Asosiy maydonlar |
|---|---|
| `users`, `profiles` | telegram_id, ism, maqsad band, imtihon sanasi, Academic/GT, til (uz/ru) |
| `items` | ko'nikma, savol turi, CEFR, `irt_b`, `irt_a`, ko'rsatilishlar soni, holat (qoralama/pilot/faol) |
| `tests`, `test_sections`, `questions` | to'liq mock tuzilmasi, javob kalitlari, qabul qilinadigan variantlar |
| `attempts`, `responses` | urinish, vaqt, har javob, avtomatik saqlash |
| `placement_results` | θ, SE, CEFR, ko'nikmalar bo'yicha taxminiy band ± oraliq |
| `writing_submissions` | matn, topshiriq, 4 mezon bo'yicha ballar, feedback JSON, model/prompt versiyasi, `human_score` (bo'lsa) |
| `speaking_submissions` | audio yo'li, transkript, so'z vaqt belgilari, fluency xususiyatlari, pronunciation natijasi, ballar |
| `cohorts`, `enrollments` | guruh, daraja, jadval, o'qituvchi, qayta guruhlash tarixi |
| `srs_cards`, `srs_reviews` | FSRS holati: so'zlar va shaxsiy xatolar |
| `payments`, `subscriptions`, `usage_quotas` | provayder, tranzaksiya, tarif, oylik limitlar |
| `org_accounts` | B2B o'quv markazlari va ularning o'quvchilari |
| `calibration_sets`, `eval_runs` | inson baholagan namunalar va har bir model/prompt versiyasi uchun QWK/MAE |

---

## 4. Baholash konveyeri

### 4.1. Listening va Reading
- Kalit bo'yicha avtomatik tekshirish: katta-kichik harf farqi yo'q, ruxsat etilgan imlo variantlari, "NO MORE THAN TWO WORDS" kabi so'z chegarasi.
- Xom ball → band: o'z konversiya jadvalimiz orqali (boshida British Council'ning taxminiy chegaralari, pilotdan keyin o'zimiznikiga o'tiladi).

### 4.2. Writing
- Claude Opus 5.5, `output_config.format` orqali qat'iy JSON sxema: `{ta_or_tr, cc, lr, gra, overall, evidence[], errors[], next_steps[]}`.
- Tizim prompti: rubrika parafrazi + har band uchun inson baholagan anchor insholar. Bu qism o'zgarmas va keshlanadi.
- Kalibratsiya to'plamida inson bilan katta farq bo'lsa yoki baholar o'zaro bir-biriga zid bo'lsa → inson navbati.

### 4.3. Speaking
1. Brauzerda `MediaRecorder` (WebM/Opus) orqali har bir javob alohida yoziladi va yuklanadi. Real vaqtli WebRTC MVP uchun kerak emas.
2. STT (verbatim, so'z vaqt belgilari bilan) → transkript.
3. Xususiyatlar: daqiqasiga so'z, pauzalar (≥0.5 s va ≥1 s), filler so'zlar, o'zini tuzatishlar.
4. Pronunciation: Azure Pronunciation Assessment (skriptsiz rejim) → tovushlar aniqligi, ravonlik, prosodiya.
5. Claude: transkript + xususiyatlar + pronunciation natijalari → 4 mezon bo'yicha JSON.

### 4.4. AI baholash sifatini nazorat qilish (eval)
- `calibration_sets`: 200 insho + 100 speaking namunasi, har biri 2 nafar mutaxassis tomonidan baholangan.
- Har bir prompt yoki model o'zgarishida avtomatik skript QWK, MAE va ±0.5 moslikni hisoblaydi. Natija oldingidan yomonlashsa → o'zgarish qabul qilinmaydi.
- Haqiqiy IELTS natijasini (TRF) yuklagan foydalanuvchilar bilan oylik "aniqlik hisoboti" saytda ochiq e'lon qilinadi.

---

## 5. Har bir birlik uchun AI narxi (🧮 hisob-kitob)

**Claude narxlari (Anthropic rasmiy, 2026-yil oktabr, 1 mln token uchun):**

| Model | Kirish | Chiqish | Kesh o'qish |
|---|---|---|---|
| Claude Opus 5.5 (`claude-opus-5-5`) | $4 | $20 | $0.20 |
| Claude Sonnet 5.5 (`claude-sonnet-5-5`) | $2 | $10 | $0.20 |
| Claude Haiku 5.5 (`claude-haiku-5-5`) | $0.10 | $0.50 | — |

Batch API — 50% chegirma (natija asinxron keladi). ✅

**Farazlar:** Writing tizim prompti ~8 000 token (keshlangan), insho va topshiriq ~800 token, chiqish (fikrlash + JSON feedback) ~4 000 token. Speaking 14 daqiqa audio. 1 dollar ≈ 12 600 so'm.

| Birlik | Hisob | Narx |
|---|---|---|
| 1 ta Writing tekshiruvi (Opus 5.5) | 8k × $0.20 + 0.8k × $4 + 4k × $20 (1 mln token uchun) | ≈ $0.085 ≈ **1 070 so'm** |
| Speaking STT (14 daq) | $0.003–0.006 / daq | ≈ $0.04–0.08 |
| Speaking Pronunciation (Azure) | ⚠️ manbalarda $0.30 dan $1.32 gacha/soat | ≈ $0.07–0.31 |
| Speaking'ni Claude bilan baholash | 4k kirish + 3k chiqish | ≈ $0.076 |
| **1 ta to'liq mock (2 Writing + Speaking), real vaqt** | 2 × 0.085 + STT + Pronunciation + 0.076 | ≈ $0.36–0.64 ≈ **4 500–8 000 so'm** |
| **1 ta to'liq mock, Batch + arzon STT/Pronunciation** | Claude qismi 50% arzon | ≈ $0.23–0.30 ≈ **2 900–3 800 so'm** |
| Listening audio yaratish (bir test, ~30 daq, ~30 000 belgi) | Azure ~$15–16 / 1 mln belgi; ElevenLabs 3–6 baravar qimmat | ≈ $0.5–3, **bir marta** |

Qaror: standart model — **Claude Opus 5.5**. Arzonroq model (masalan, Sonnet 5.5) faqat kalibratsiya to'plamida sifat bir xil chiqqanini o'lchagandan keyin va **sizning qaroringiz bilan** ishlatiladi. Haqiqiy narx `usage` maydonlari orqali kuzatiladi: bu jadval faqat boshlang'ich taxmin.

---

## 6. STT tanlash testi (qurishdan oldin 1 kun)

Yaroqsiz STT Speaking baholashni ko'r qiladi, shuning uchun:
1. O'zbek aksentli 20 ta yozuv yig'ing (5.0 dan 8.0 gacha bo'lgan turli darajalar).
2. Ularni qo'lda so'zma-so'z transkript qiling: barcha "um", takror va xatolar bilan.
3. 2–3 ta nomzod STT'ni solishtiring: WER (so'z xatosi), filler so'zlar saqlanish foizi, vaqt belgilari aniqligi, jimlikda "o'ylab topilgan" gaplar soni, narx.
4. Eng yaxshisini tanlang va natijani shu papkaga yozib qo'ying.

📚 Whisper oilasidagi modellar filler so'zlarni ko'pincha olib tashlaydi va jimlikda gap "o'ylab topishi" mumkin (Koenecke va boshq., 2024). Shuning uchun Whisper (jumladan Groq orqali) faqat shu testdan o'tsa ishlatiladi.

---

## 7. Claude Code sozlamasi

### 7.1. Hozir repoda nima bor

`.claude/settings.json` `hormozi-brain` plaginini yoqadi (manba: `aziznizomofficial/hormozi-brain`, uchinchi tomon repozitoriysi). Bu sessiyada plagin o'rnatilmagan. Undan taklif (offer) dizayni uchun foydalanish mumkin, lekin ishonishdan oldin uning tarkibini ko'rib chiqing.

### 7.2. O'rnatiladigan narsalar (qurish boshlanganda)

| Nima | Qayerda | Vazifasi |
|---|---|---|
| `CLAUDE.md` | repo ildizi | Loyiha qoidalari: har sessiyada yuklanadi (namuna pastda) |
| Playwright MCP | `claude mcp add playwright -- npx @playwright/mcp@latest` | Claude imtihon interfeysini brauzerda o'zi sinab ko'radi |
| Context7 MCP | `claude mcp add context7 -- npx -y @upstash/context7-mcp` | Next.js, Drizzle va boshqa kutubxonalarning yangi hujjatlari |
| Ma'lumotlar bazasi MCP | Supabase MCP yoki Postgres MCP | Sxemani ko'rish va so'rovlarni tekshirish (faqat dev baza!) |
| GitHub MCP | allaqachon ulangan | PR va issue'lar |
| Skill: `ielts-content` | `.claude/skills/ielts-content/SKILL.md` | Original matn va savol yozish qoidalari, CEFR teglari, tekshiruv ro'yxati |
| Skill: `scoring-eval` | `.claude/skills/scoring-eval/SKILL.md` | Kalibratsiya skriptini ishga tushirish va QWK/MAE natijasini o'qish |
| Subagent: `content-reviewer` | `.claude/agents/content-reviewer.md` | Har savolni tekshiradi: bitta to'g'ri javob, javob matnda bor, noaniqlik yo'q |
| Subagent: `qa-tester` | `.claude/agents/qa-tester.md` | Playwright bilan to'liq mock jarayonini sinaydi |
| Hook: tahrirdan keyin | `.claude/settings.json` → `PostToolUse` (Edit/Write) | `lint` + `typecheck` avtomatik |
| Hook: himoya | `PreToolUse` | `.env` va migratsiya fayllarini ruxsatsiz o'zgartirishni bloklash |

📚 MCP paket nomlarini o'rnatishdan oldin ularning rasmiy sahifasida tekshiring: ular o'zgarib turadi.

### 7.3. `CLAUDE.md` namunasi

```markdown
# Hormoz — ingliz tili mock va tayyorlov platformasi

## Mahsulot
- IELTS uslubidagi kompyuter imtihoni simulyatori + placement + darajali o'qitish.
- Brend nomida va UI'da "IELTS" logotipi ishlatilmaydi. Har sahifa pastida disclaimer bor.
- Barcha test kontenti original. Cambridge kitoblaridan yoki real imtihonlardan matn qo'shma.
- Batafsil reja: docs/ papkasi (01–05).

## Stek
- Next.js (App Router) + TypeScript (strict) + Tailwind + shadcn/ui
- PostgreSQL + Drizzle ORM; migratsiyalar faqat `drizzle-kit generate` orqali
- Claude API: model `claude-opus-5-5`, structured outputs, rubrika qismi keshlanadi
- Auth: Telegram Login

## Qoidalar
- API kalitlari faqat serverda. Klient kodida hech qanday maxfiy kalit bo'lmasin.
- Har bir AI ball bilan model_id, prompt_version, rubric_version saqlanadi.
- Ballarni hisoblash (xom → band, yaxlitlash, Task 2 og'irligi) faqat `lib/scoring/` da, unit testlar bilan.
- Imtihon jarayoniga tegadigan har o'zgarishdan keyin Playwright E2E testi ishga tushiriladi.
- Prompt yoki model o'zgarsa, `npm run eval:scoring` ishga tushiriladi; QWK pasaysa, o'zgarish qabul qilinmaydi.
- UI matnlari o'zbek (lotin) va rus tilida; i18n kalitlari orqali.

## Buyruqlar
- `npm run dev` / `npm test` / `npm run test:e2e` / `npm run lint` / `npm run typecheck` / `npm run eval:scoring`
```

### 7.4. Claude Code bilan ishlash tartibi

1. Har bir funksiya alohida sessiyada yoki alohida PR'da qilinadi.
2. Sessiyani **Plan mode**'da boshlang: "docs/05 dagi 2-bosqich, 3-vazifani rejalashtir. docs/02 dagi qoidalarga amal qil."
3. Rejani tasdiqlaganingizdan keyin Claude kod yozadi, test yozadi va testlarni ishga tushiradi.
4. Oxirida `/code-review` va kerak bo'lsa `/security-review`.
5. Commit → push → PR.

---

## Manbalar

- [Claude Code — Features overview](https://code.claude.com/docs/en/features-overview)
- [Claude Code — Plugins reference](https://code.claude.com/docs/en/plugins-reference)
- Claude API narxlari va model ID'lari: Anthropic rasmiy hujjatlari (claude-api skill, 2026-10-06 holatiga kesh)
- [STT narxlarini solishtirish (Soniox)](https://soniox.com/compare/stt/pricing), [Deepgram narxlari sharhi](https://www.happyrobot.ai/hub/deepgram-pricing), [ElevenLabs Scribe narxlari](https://costbench.com/software/ai-transcription-apis/elevenlabs-scribe/), [Speech-to-Text 2026 taqqoslash](https://www.web3aiblog.com/blog/speech-to-text-models-compared-whisper-deepgram-assemblyai-elevenlabs-2026)
- [Azure Pronunciation Assessment narxi (Microsoft Q&A)](https://learn.microsoft.com/en-us/answers/questions/5608069/pricing-and-usage-of-pronunciation-assessment-feat), [Azure Speech narxlari](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services)
- [TTS narxlarini solishtirish 2026](https://blogs.novita.ai/best-text-to-speech-api-2026/), [Voice API pricing 2026](https://costbench.com/stats/voice-api-pricing-2026/)
- [Click merchant API](https://paymentproviders.io/providers/click-uzb.md), [Uzcard/Humo integratsiya qo'llanmasi](https://sayt.uz/en/blog/uzcard-humo-tolov-integratsiya), [Spot.uz — QR to'lov komissiyalari 1–3%](https://www.spot.uz/ru/2026/06/23/uzqr-fees/), [Kun.uz — O'zbekistonda to'lov bozori](https://kun.uz/en/news/2026/05/19/how-do-people-in-uzbekistan-pay-and-who-leads-the-market)
