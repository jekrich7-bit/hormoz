# 06 — Siz yuborgan rejaning tahlili

Savol: "Bularning barchasi to'g'rimi, foydalanishga arziydimi?"

**Qisqa javob:** skelet sifatida arziydi. Tuzilishi (stek → baholash → darajalar → qadamlar → biznes) to'g'ri yo'nalishda va taxminan 60 foizi bizning rejaga mos keladi. Lekin unda **6 ta jiddiy xato** bor. Ularni tuzatmasdan qurilsa, kod ishlamaydi (eski model), Speaking noto'g'ri baholanadi yoki tarif zarar keltiradi. Quyida har band bo'yicha baho.

Belgilar: ✅ to'g'ri · ⚠️ qisman to'g'ri, tuzatish kerak · ❌ noto'g'ri yoki eskirgan

---

## 1. Texnologik stek

| Taklif | Baho | Izoh va tuzatish |
|---|---|---|
| Next.js 15 + Tailwind + shadcn/ui | ⚠️ | Tanlov to'g'ri, lekin 15 eski versiya. `create-next-app@latest` ishlating |
| Supabase (Postgres + Auth + Storage + RLS) | ⚠️ | MVP uchun yaxshi. Lekin O'zbekistonda shaxsiy ma'lumotlarni lokalizatsiya qilish talabi bor 📚, Supabase'da esa O'zbekiston regioni yo'q. Drizzle + oddiy Postgres bilan yozing, keyin ko'chirish oson bo'ladi. "Realtime" imtihon uchun kerak emas |
| STT: Groq Whisper-large-v3 yoki Deepgram Nova-2, "~300ms, real vaqt" | ❌ | (1) Real vaqt kerak emas: javob yozib bo'lingach transkript qilish yetarli. (2) **Asosiy muammo:** Whisper oilasi "um", takror va pauzalarni ko'pincha o'chirib yuboradi, jimlikda esa gap "o'ylab topishi" mumkin 📚. Fluency'ni aynan shular bilan o'lchaysiz. (3) Nova-2 eski, hozir Nova-3 bor. **Tuzatish:** verbatim STT, so'z vaqt belgilari bilan; tanlovni 20 ta o'zbek aksentli yozuvda sinab qiling (03-hujjat, 6-bo'lim) |
| TTS: ElevenLabs yoki OpenAI tts-1 | ⚠️ | ElevenLabs yaxshi tanlov. ⚠️ Bitta manbaga ko'ra OpenAI speech endpoint modellari 2026-yil 1-oktabrda eskirgan deb e'lon qilingan va 2027-yil 6-yanvarda o'chiriladi, shuning uchun ularga tayanmang. Arzon alternativa: Azure Neural (~$15–16 / 1 mln belgi). Audioni keshlash g'oyasi ✅ |
| Baholash: Claude 3.5 Sonnet yoki GPT-4o-mini | ❌ | Claude 3.5 Sonnet iste'moldan chiqarilgan (retired) 📚, API uni qabul qilmaydi: kod darhol xato beradi. Hozirgi modellar: **Claude Opus 5.5** (`claude-opus-5-5`, baholash uchun standart), Sonnet 5.5, Haiku 5.5. Kichik va eski modellar IELTS insholariga **pastroq** ball qo'yishga moyil ✅ |
| WebRTC / MediaRecorder | ✅/⚠️ | MediaRecorder to'g'ri. WebRTC jonli suhbat bo'lmasa kerak emas |
| **Talaffuz vositasi** | ❌ yo'q | 2-bo'limda "akustik model" deyilgan, lekin stekda unday vosita yo'q. STT matni talaffuz xatolarini yashiradi. **Qo'shing:** Azure Pronunciation Assessment yoki SpeechSuper/SpeechAce |

## 2. Baholash metodologiyasi

| Taklif | Baho | Izoh |
|---|---|---|
| "CEFR va band descriptor'larga **qat'iy** mos hisoblaydi" | ❌ | Bu ortiqcha va'da. Rasmiy bo'lmagan platforma faqat **taxminiy band ± oraliq** bera oladi. Huquqiy va axloqiy jihatdan ham shunday yozish kerak |
| L/R: 40 savol, xom ball → band; Academic Reading 30/40 = 7.0, 35/40 = 8.0 | ✅ | British Council chegaralariga taxminan mos ✅. Qo'shing: chegaralar test versiyasiga qarab o'zgaradi; **General Training Reading jadvali boshqacha** (30/40 ≈ 6.0) |
| Writing: 4 mezon, har biri 25% | ✅ | Qo'shing: **Task 2 Task 1 dan ikki baravar og'ir**, umumiy bandni yaxlitlash qoidalari |
| "Promptga public band descriptor matritsasi kiritiladi" | ⚠️ | Yetarli emas. Tadqiqotlar ko'rsatadiki, LLM va inson mosligi kontekstga juda bog'liq va model yangilanganda o'zgaradi ✅. Kerak: inson baholagan anchor insholar, kalibratsiya to'plami, QWK/MAE o'lchovi, inson navbati |
| Speaking FC: WPM, pauzalar, filler so'zlar | ✅ | Yaxshi xususiyatlar, lekin faqat verbatim STT bilan ishlaydi (yuqoriga qarang). FC faqat tezlik emas, fikr izchilligi ham |
| LR: "C1/C2 so'zlar ulushi va idiomalar" | ⚠️ | Faqat yordamchi ko'rsatkich. IELTS so'zlarning **o'rinli va aniq** ishlatilishini baholaydi; yodlangan, joyiga tushmagan idioma ball pasaytiradi |
| Pronunciation: "fonema darajasida (intonatsiya, urg'u)" | ⚠️ | G'oya to'g'ri, lekin intonatsiya va urg'u fonema emas, prosodiya. IELTS aksentni emas, tushunarlilikni baholaydi |

## 3. Darajalar va guruhlash

| Taklif | Baho | Izoh |
|---|---|---|
| 5.0 = B1 "Modest", 6.0 = B2 "Competent", 7.0 = C1 "Good" | ✅ | IELTS'ning rasmiy jadvaliga mos ✅ |
| "8.0–9.0 = C2, Expert User" | ❌ | 8.0 — C1 ning yuqori qismi va "Very good user". C2 8.5 dan boshlanadi ✅. Faqat 9.0 "Expert user" |
| 5.0 dan past o'quvchilar | ❌ yo'q | Ko'p o'quvchi B1 dan past boshlaydi. Foundation (A1–A2) va Pre-IELTS yo'llari kerak (02-hujjat) |
| Band 5: "asosiy 2000 ta akademik so'z" | ⚠️ | B1 da avval eng ko'p ishlatiladigan 2 000–3 000 **umumiy** so'z kerak. Academic Word List 570 so'z oilasidan iborat va keyinroq o'rganiladi 📚 |
| Band 7: "deyarli xatosiz gapiradi/yozadi" | ⚠️ | 7 da xatosiz gaplar **ko'p**, lekin xatolar hali bor. "Deyarli xatosiz" — bu 8–9 |
| Band 8–9: "native-level", "madaniy kontekst" | ⚠️ | IELTS native bo'lishni yoki madaniy bilimni talab qilmaydi; talab — to'liq nazorat va aniqlik |
| Avtomatik guruhlash | ⚠️ | G'oya to'g'ri, lekin dalillar ko'rsatadiki, faqat guruhlashning o'zi deyarli natija bermaydi (g = 0.04–0.06) ✅. Moslashtirilgan kontent va har 4 haftada qayta guruhlash bilan birga ishlatilishi kerak |

## 4. Claude Code qadamlari

| Taklif | Baho | Izoh |
|---|---|---|
| `npx create-next-app@latest ... --typescript --tailwind --app`, keyin `claude` | ✅ | Buyruq to'g'ri. Loyihani shu `hormoz` reposining ichida yarating |
| 4 ta katta prompt | ⚠️ | Har biri juda katta va qabul mezoni yo'q. Yetishmaydi: `CLAUDE.md`, Plan mode, testlar, Playwright, kontent ishlab chiqarish, auth, to'lov, placement, eval. Yaxshilangan ketma-ketlik 05-hujjatda: har vazifaning qabul mezoni bilan |
| "Validates against official descriptors" | ❌ | Kalibratsiya to'plamisiz baholash sifatini bilib bo'lmaydi |
| **Kontent qayerdan olinadi?** | ❌ yo'q | Eng katta ish hajmi va eng katta huquqiy xavf. Faqat original kontent (02-hujjat, 8-bo'lim) |

## 5. Biznes va moliya

| Taklif | Baho | Izoh |
|---|---|---|
| 1 ta mock tannarxi ~$0.08–0.12 (1 200–1 600 so'm) | ⚠️ | Eski va arzon model hamda talaffuz bahosisiz hisoblangan. Hozirgi narxlarda va Pronunciation bilan: **$0.23–0.64 (≈ 2 900–8 000 so'm)** 🧮 (03-hujjat). Biznes baribir foydali |
| "O'quv markazlarida jonli mock 60 000–150 000 so'm" | ⚠️ | Manba topilmadi. Bundan ham muhimi: haqiqiy raqobat **onlayn**, u yerda 1 test 9 000–19 000 so'm, ko'pi bepul ✅ |
| 1 mock 29 000 so'm, "marja >90%" | ⚠️ | Narx yaxshi; real marja 70–85% 🧮 |
| 99 000 so'm/oy, "cheksiz Writing check" | ❌ | Har bir tekshiruvning tannarxi bor (~1 070 so'm). "Cheksiz" tarif faol foydalanuvchilarda va akkaunt almashilganda zararga aylanadi. Oylik limit qo'ying (04-hujjat, 3.2) |
| B2B: 500 tekshiruv = 2 500 000 so'm | ✅ | Yaxshi taklif, marja ≈ 79% 🧮 |
| Yetishmaydi | ❌ | Marketing (CAC), doimiy xarajatlar, kontent va kalibratsiya xarajati, soliq, huquqiy masalalar va eng kuchli dalil: **7.0 uchun davlat xarajatni qaytaradi** ✅ |

---

## Yakuniy xulosa

- **Foydalanish mumkin:** umumiy tuzilma, Next.js + Tailwind + shadcn, MediaRecorder, L/R konversiya g'oyasi, 4 mezonli baholash, Speaking xususiyatlari, 29 000 so'mlik mock, B2B paketi.
- **Albatta almashtiring:** Claude 3.5 Sonnet / GPT-4o-mini → Claude Opus 5.5; Whisper → sinovdan o'tgan verbatim STT; talaffuz vositasini qo'shing; "cheksiz" tarif → limitli tarif; "qat'iy mos" → "taxminiy band ± oraliq + ochiq aniqlik hisoboti"; 8.0 = C1.
- **Qo'shing:** kalibratsiya to'plami va eval, original kontent rejasi, huquqiy masalalar, 5.0 dan past darajalar, ilmiy asoslangan o'qitish sikli.

Bularning barchasi 01–05-hujjatlarda dalillari bilan hisobga olingan.
