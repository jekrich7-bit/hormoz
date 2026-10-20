# 5. Yo'l xaritasi: 16 hafta

**Faraz:** siz asosan yolg'iz ishlaysiz, Claude Code bilan, kuniga 6-8 soat. Agar jamoa bo'lsa, Faza 2 va Faza 3 parallel yuradi va jami muddat ~10 haftaga qisqaradi.

```
Hafta:   0    1    2    3    4    5    6    7    8    9   10   11   12   13   14   15   16
Faza 0  ██
Faza 1       ████████
Faza 2                 ████████████
Faza 3                               ████████████████████
Faza 4                                                    ████████████
Faza 5                                                                  ████████████
Launch:                          Studio beta ▲                  Motion beta ▲      Ommaviy ▲
```

| Faza | Muddat | Natija |
|---|---|---|
| 0. Tayyorgarlik | 3-5 kun | Akkauntlar, yuridik shaxs, to'lov arizalari, brend |
| 1. Yadro | Hafta 1-2 | Monorepo, auth, baza, kredit ledger, worker, 1 model boshidan oxirigacha ishlaydi |
| 2. Studio MVP | Hafta 3-5 | Studio yopiq beta (50 kishi), birinchi to'lovlar |
| 3. Motion MVP | Hafta 6-10 | Motion yopiq beta |
| 4. Integratsiya va o'sish | Hafta 11-13 | Umumiy hamyon, B-roll, scene builder, personajlar, referral, mahalliy shablonlar |
| 5. Kanallar va B2B | Hafta 14-16 | Telegram bot va Mini App, API, ommaviy launch |

---

## Faza 0: Tayyorgarlik (3-5 kun)

- [ ] **Yuridik shaxs:** MChJ ochish (yoki boshlanishda YaTT). IT Park rezidentligi uchun ariza (6-hujjatga qarang)
- [ ] **To'lov tizimlari:** Payme Business, Click va Uzum merchant arizalari. Odatda kerak bo'ladi: yuridik shaxs, sayt, **ommaviy oferta**, maxfiylik siyosati va mahsulot tavsifi. ⏳ Eng uzoq kutiladigan qism shu
- [ ] **Akkauntlar:** Anthropic API, Google AI Studio yoki Vertex (Veo), fal.ai, Cloudflare (R2, DNS), AWS (Lambda), Vercel, Trigger.dev, Neon yoki Supabase, Upstash, Sentry, PostHog, Langfuse, Resend, SMS shlyuz
- [ ] **Xalqaro karta:** provayderlarga USD'da to'lash uchun korporativ Visa yoki Mastercard
- [ ] **Brend:** nom, 2 ta domen (studio va motion), logo, rang palitrasi
- [ ] **Hujjatlar:** foydalanish shartlari, maxfiylik siyosati, kontent siyosati, oferta (uz/ru)
- [ ] **Raqobatchilarni sinash:** Higgsfield, Flow va Claude Motion'da 1 kun ishlab, skrinshot va izohlar yig'ing

## Faza 1: Yadro (Hafta 1-2)

### Hafta 1
- [ ] Monorepo (pnpm, Turborepo), `apps/studio`, `apps/worker`, `packages/{db,auth,credits,providers,storage,ui,i18n}`
- [ ] CI: lint, typecheck, test (GitHub Actions)
- [ ] Drizzle schema (2-hujjat, 2.4-bo'lim) va migratsiyalar
- [ ] Better Auth: Google va email. Telegram Login va telefon OTP keyingi haftada
- [ ] `packages/credits`: reserve, capture, release va grant + **to'liq testlar** (parallellik, idempotentlik)
- [ ] R2: yuklash, signed URL, ffprobe metadata va thumbnail

**Tayyor bo'ldi:** testlar yashil, lokal muhitda foydalanuvchi ro'yxatdan o'tadi va unga kredit beriladi.

### Hafta 2
- [ ] Trigger.dev worker: `generate` task (submit, webhook yoki poll, R2'ga ko'chirish, capture yoki release)
- [ ] `packages/providers`: Gemini (Veo 3.1 Fast) va fal (1 ta rasm modeli) adapterlari
- [ ] Model katalogi bazada, seed skript bilan
- [ ] Realtime holat (SSE)
- [ ] Telegram Login va telefon OTP
- [ ] Admin panel (minimal): foydalanuvchilar, balans, generatsiyalar, kredit berish
- [ ] Sentry, PostHog va Langfuse ulanadi

**Tayyor bo'ldi:** ataylab xato qilingan generatsiyada kredit avtomatik qaytadi. Bitta Veo video boshidan oxirigacha ishlaydi va R2'da saqlanadi.

## Faza 2: Studio MVP (Hafta 3-5)

### Hafta 3: Yaratish tajribasi
- [ ] Create ekrani: prompt, model kartalari, format, davomiylik, referens rasm, aniq kredit narxi
- [ ] Prompt enhancer (structured outputs) va moderatsiya
- [ ] Qo'shimcha modellar: Veo 3.1 Lite, Kling 3.0, Seedance (fal) va 2-rasm modeli
- [ ] Natija ekrani va Library

### Hafta 4: Presetlar va to'lov
- [ ] Preset dvigateli va 20 ta preset (namunaviy videolarni o'zingiz yaratasiz, bu ayni paytda marketing kontenti ham)
- [ ] Explore lentasi
- [ ] **Payme va Click integratsiyasi** (sandbox, keyin real), kredit paketlari
- [ ] Bepul tarif qoidalari: 10 kredit, telefonsiz video yo'q, watermark

### Hafta 5: Sayqal va beta
- [ ] Landing sahifa (uz/ru), narxlar sahifasi, FAQ
- [ ] Mobil versiyani sayqallash
- [ ] Rate limit va suiiste'molga qarshi choralar
- [ ] **Yopiq beta: 50 kishi** (SMM menejerlar, kontent yaratuvchilar). Har biriga 50 kredit beriladi, evaziga fikr so'raladi
- [ ] Har kuni: xatolarni ko'rish, 3 ta eng ko'p shikoyatni tuzatish

**Studio MVP tayyor:** 3-hujjatdagi 3.12-bo'lim mezonlari bajarilgan, birinchi 10 ta pullik to'lov amalga oshgan.

## Faza 3: Motion MVP (Hafta 6-10)

### Hafta 6: Dvigatel
- [ ] `apps/motion` va `packages/motion-engine`
- [ ] DSL sxemasi (zod), `defineScene` shartnomasi, mavzu tokenlari
- [ ] Remotion Player bilan preview sahifasi (JSON'ni qo'lda yozib sinash)
- [ ] Birinchi 5 ta komponent: KineticTitle, StatCounter, FeatureList, CTAEndCard, LowerThird va 2 ta o'tish

### Hafta 7: Komponentlar zavodi
- [ ] Yana 12 ta komponent. **Har birini alohida Claude Code sessiyasida, parallel ishlang** (har komponentning shartnomasi aniq, shuning uchun ular bir-biriga bog'liq emas)
- [ ] Storybook uslubidagi "komponentlar galereyasi" sahifasi: har komponent 3 formatda va 6 mavzuda
- [ ] Shrift va glif testlari (oʻ, gʻ, kirill)

### Hafta 8: Director
- [ ] Director prompti, structured output va stream
- [ ] Composer: validatsiya, fitText, kontrast, xavfsiz zonalar, davomiylik
- [ ] **Eval to'plami:** 50 ta so'rov va avtomatik o'lchov skripti
- [ ] Qolgan 8 ta komponent (jami 25 ta)

### Hafta 9: Ovoz, musiqa va tahrir
- [ ] TTS (uz, ru, en), so'z vaqtlari va karaoke subtitrlar
- [ ] Musiqa kutubxonasi va beat sinxronlash
- [ ] Forma orqali tahrir (zod'dan avtomatik) va chat orqali tahrir (JSON Patch), undo
- [ ] Brend to'plami

### Hafta 10: Render va beta
- [ ] Remotion Lambda deploy, render navbati, R2 va kesh
- [ ] Kredit narxi (10 kredit / 30 s), umumiy hamyon
- [ ] Landing, namunalar galereyasi (30 ta video)
- [ ] **Motion yopiq beta: 50 kishi** (SMM agentliklar, kichik bizneslar)

**Motion MVP tayyor:** 4-hujjatdagi 4.13-bo'lim mezonlari bajarilgan.

## Faza 4: Integratsiya va o'sish (Hafta 11-13)

- [ ] Studio va Motion o'rtasida SSO va umumiy kredit hamyoni, bitta billing sahifasi
- [ ] Studio klipini Motion'da B-roll sifatida ishlatish, Motion'dan Studio'ga "AI fon yaratish" tugmasi
- [ ] Studio: Scene builder (v2) va Characters (yurist bilan kelishilgandan keyin)
- [ ] Motion: sayt URL'dan promo, CSV'dan infografika, mahsulot rasmidan reklama
- [ ] **Mahalliy shablonlar:** to'y, bayram, Uzum mahsuloti, ko'chmas mulk, o'quv markazi
- [ ] Referral: taklif qilgan va taklif qilingan odamga 30 kreditdan
- [ ] Obuna tariflari (Payme karta tokenizatsiyasi orqali avtomatik to'lov, imkoniyatini tekshiring)

## Faza 5: Kanallar va B2B (Hafta 14-16)

- [ ] **Telegram bot** (grammY): post yuborilsa Motion video qaytadi, rasm yuborilsa Studio preset ishlaydi, to'lov Telegram Stars orqali
- [ ] **Telegram Mini App**: Studio va Motion'ning yengil versiyasi
- [ ] **Public API** va API kalitlar, ommaviy (bulk) generatsiya, webhook
- [ ] Jamoa (workspace), "Agency" tarifi
- [ ] Ingliz tili va xalqaro to'lov (MoR), Product Hunt'ga chiqish
- [ ] **Ommaviy launch** (6-hujjatdagi marketing rejasiga qarang)

---

## KPI va maqsadlar

| Ko'rsatkich | Maqsad | Qayerda o'lchanadi |
|---|---|---|
| Aktivatsiya: ro'yxatdan o'tgan va 10 daqiqada birinchi natijani olgan | ≥ 60% | PostHog funnel |
| Bepuldan pullikka o'tish | 3-6% | PostHog va payments |
| Generatsiya muvaffaqiyati | ≥ 97% | generations.status |
| Motion preview p50 | ≤ 45 s | Langfuse va o'z metrikamiz |
| Motion MP4 p50 | ≤ 2 daqiqa | render logi |
| Yalpi marja (Studio / Motion) | ≥ 55% / ≥ 75% | ledger va cost_usd_actual |
| Pullik foydalanuvchilarning 1-oy retention'i | ≥ 50% | payments |
| Shikoyat va qaytarish | < 2% | admin |

**Har hafta juma kuni** 30 daqiqa ajrating: KPI'larni ko'rib chiqing, eng ko'p uchragan 3 ta xatoni va 3 ta foydalanuvchi so'rovini tanlang, keyingi hafta shularni tuzating.

## Scope'ni qisqartirish qoidasi

Agar reja 2 haftadan ko'proq orqada qolsa, quyidagi tartibda **qisqartiring** (hech qachon kredit testlari va moderatsiyadan emas):
1. Characters va Scene builder'ni keyinga suring
2. Komponentlarni 25 tadan 18 taga, mavzularni 6 tadan 4 taga tushiring
3. Uzum to'lovini keyinga suring (Payme va Click yetadi)
4. Custom code rejimini butunlay olib tashlang (u allaqachon v2'da)
