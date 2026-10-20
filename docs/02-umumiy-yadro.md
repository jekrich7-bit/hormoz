# 2. Umumiy yadro: arxitektura va texnologiyalar

Ikkala platforma bitta yadroni ishlatadi. Yadroni bir marta to'g'ri qurish ikkinchi mahsulotni 2 baravar tez chiqarishga imkon beradi.

## 2.1. Yuqori darajadagi arxitektura

```
   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
   │ studio (web) │   │ motion (web) │   │ admin (web)  │   │ Telegram bot │
   └──────┬───────┘   └──────┬───────┘   └──────┬───────┘   └──────┬───────┘
          └──────────────────┴─────────┬────────┴──────────────────┘
                                       ▼
                 API qatlam (Next.js route handlers + zod validatsiya)
            auth · kredit · loyihalar · assetlar · to'lov · webhooklar
                                       │
          ┌────────────────────────────┼─────────────────────────────┐
          ▼                            ▼                             ▼
   Postgres (Drizzle)        Job orkestrator                Cloudflare R2 + CDN
   users, wallets, ledger,   (Trigger.dev yoki Inngest)     video, rasm, audio,
   generations, assets...    retry, timeout, webhook kutish thumbnail
                                       │
     ┌───────────────┬─────────────────┼─────────────────┬──────────────────┐
     ▼               ▼                 ▼                 ▼                  ▼
 Video modellar   Rasm modellar     LLM (Claude)       TTS (ovoz)       Render
 Veo (Gemini API) Nano Banana,      director, prompt   Azure uz-UZ,     Remotion Lambda
 Kling, Seedance  Seedream, Flux    enhancer,          ElevenLabs       yoki o'z
 (fal / to'g'ri)  (fal / to'g'ri)   moderatsiya                         workerlarimiz
```

**Asosiy tamoyil:** web ilova hech qachon uzoq ishni o'zi kutmaydi. U faqat vazifani (job) yaratadi, kreditni band qiladi va darhol `generationId` qaytaradi. Qolganini worker bajaradi, holat esa real vaqtda (SSE yoki realtime) foydalanuvchiga uzatiladi.

## 2.2. Tech stack (tavsiya)

| Qatlam | Tanlov | Nega |
|---|---|---|
| Monorepo | **pnpm + Turborepo** | Bir nechta ilova va umumiy paketlar, tez build |
| Frontend | **Next.js (App Router) + TypeScript + Tailwind + shadcn/ui** | Claude Code eng yaxshi biladigan stack, vibe-coding uchun ideal |
| Holat (state) | TanStack Query (server holati) + Zustand (editor holati) | Editor uchun oddiy va tez |
| Baza | **Postgres** (Neon yoki Supabase) + **Drizzle ORM** | Tranzaksiyalar kredit tizimi uchun shart |
| Job orkestratsiya | **Trigger.dev** (yoki Inngest) | Retry, timeout, webhook kutish, kuzatuv tayyor holda keladi. O'zingiz host qilmoqchi bo'lsangiz BullMQ + Redis |
| Storage | **Cloudflare R2** + Cloudflare CDN | **Egress bepul.** Video uchun bu juda katta tejash (S3'da trafik qimmat) |
| Auth | **Better Auth** | Bepul va o'zingizda turadi. Google, Telegram Login va telefon OTP qo'shish oson |
| SMS (OTP) | Eskiz.uz yoki shunga o'xshash mahalliy SMS shlyuz | O'zbek raqamlari uchun arzon va ishonchli |
| To'lov (UZ) | **Payme, Click, Uzum** | Mahalliy kartalar (Uzcard, Humo) |
| To'lov (xalqaro) | Paddle / Polar / Lemon Squeezy / Dodo (MoR) | O'zbekistonlik sotuvchini qabul qilishini **avval tekshiring** |
| To'lov (Telegram) | **Telegram Stars** | Bot yoki Mini App ichida raqamli mahsulot sotish uchun Telegram qoidasi shuni talab qiladi |
| Realtime | SSE yoki Trigger.dev Realtime | Generatsiya holatini jonli ko'rsatish |
| LLM | **Claude API** (`@anthropic-ai/sdk`) | Structured outputs, prompt caching |
| Kuzatuv | Sentry (xatolar), PostHog (analitika, feature flag), Langfuse (LLM trace) | Boshlanishda bepul tariflar yetadi |
| Rate limit | Upstash Redis + `@upstash/ratelimit` | Suiiste'molga qarshi |
| Email | Resend | Kvitansiya va bildirishnomalar |
| Hosting | Vercel (web) + Railway/Fly.io/Hetzner (worker, ffmpeg) + AWS (Remotion Lambda) | Web serverless, og'ir ishlar alohida |
| Telegram bot | grammY (TypeScript) | Eng qulay TS freymvork |

## 2.3. Monorepo tuzilmasi

```
hormoz/
├─ apps/
│  ├─ studio/            # Platforma A (Next.js)
│  ├─ motion/            # Platforma B (Next.js)
│  ├─ admin/             # ichki panel: foydalanuvchilar, kreditlar, xatolar, modellar katalogi
│  ├─ worker/            # Trigger.dev tasklari: generatsiya, render, webhook qayta ishlash, ffmpeg
│  └─ bot/               # Telegram bot (grammY), Faza 5
├─ packages/
│  ├─ db/                # Drizzle schema + migratsiyalar
│  ├─ auth/              # Better Auth konfiguratsiyasi
│  ├─ credits/           # kredit ledger: reserve / capture / release. TESTLARSIZ MERGE QILINMAYDI
│  ├─ billing/           # Payme, Click, Uzum, MoR va Telegram Stars adapterlari
│  ├─ providers/         # video/rasm/TTS/LLM adapterlari + model katalogi tiplari
│  ├─ storage/           # R2 upload, signed URL, thumbnail, metadata (ffprobe)
│  ├─ motion-engine/     # Remotion kompozitsiyalari, sahna sxemalari (zod), mavzular
│  ├─ prompts/           # LLM promptlari (versiyalangan) + eval to'plamlari
│  ├─ moderation/        # prompt va rasm moderatsiyasi
│  ├─ ui/                # umumiy komponentlar, dizayn tokenlari
│  └─ i18n/              # uz-Latn, uz-Cyrl, ru, en
├─ docs/
└─ CLAUDE.md
```

## 2.4. Baza sxemasi (asosiy jadvallar)

```
users            id, email, phone, telegram_id, name, locale, role, created_at, banned_at
workspaces       id, owner_id, name, plan, created_at            -- jamoa uchun (keyinroq)
wallets          id, workspace_id, balance_cached, updated_at    -- tez o'qish uchun kesh
credit_ledger    id, wallet_id, type, amount, generation_id, payment_id,
                 expires_at, idempotency_key UNIQUE, meta jsonb, created_at
                 -- type: GRANT_SUBSCRIPTION | PURCHASE | BONUS | REFERRAL |
                 --       HOLD | CAPTURE | RELEASE | REFUND | EXPIRE | ADMIN_ADJUST
generations      id, workspace_id, user_id, product ('studio'|'motion'),
                 kind ('image'|'video'|'tts'|'motion_render'|...),
                 model_id, provider, params jsonb, prompt, enhanced_prompt,
                 status, credits_held, credits_charged, cost_usd_estimate, cost_usd_actual,
                 provider_job_id, error_code, error_message, attempts,
                 started_at, finished_at, created_at
assets           id, workspace_id, generation_id, kind, r2_key, mime, width, height,
                 duration_ms, size_bytes, thumb_key, is_public, created_at
projects         id, workspace_id, product, title, data jsonb, updated_at
                 -- Studio: scene builder timeline;  Motion: sahna grafi (DSL)
characters       id, workspace_id, name, description, ref_asset_ids[], consent_at
presets          id, slug, product, category, title jsonb, config jsonb, example_asset_id,
                 is_active, sort, created_at
models           id, provider, provider_model, kind, capabilities jsonb, pricing jsonb,
                 markup, is_active, fallback_model_id
payments         id, workspace_id, provider, provider_tx_id UNIQUE, amount_uzs,
                 amount_usd, status, credits, raw jsonb, created_at
subscriptions    id, workspace_id, plan, status, period_start, period_end, provider
webhook_events   id, source, external_id UNIQUE, payload jsonb, processed_at
                 -- har bir webhook bir marta qayta ishlanishi uchun
```

## 2.5. Kredit tizimi (eng muhim qism)

### Tamoyillar
1. **Ledger faqat qo'shiladi (append-only).** Hech qachon UPDATE yoki DELETE qilinmaydi. Balans ledger yozuvlarining yig'indisiga teng, `wallets.balance_cached` esa faqat tez o'qish uchun.
2. **Avval band qilish (HOLD), keyin yechish (CAPTURE).** Generatsiya boshlanishidan oldin kredit band qilinadi. Muvaffaqiyatli bo'lsa yechiladi, xato bo'lsa **avtomatik qaytariladi** (RELEASE). Bu foydalanuvchiga beriladigan kafolat ham bo'ladi.
3. **Idempotentlik.** Har bir yozuvda `idempotency_key` bor, shuning uchun qayta yuborilgan webhook yoki retry ikki marta pul yechmaydi.
4. **Tranzaksiya va qulf.** `SELECT ... FROM wallets WHERE id = $1 FOR UPDATE`. Ikki parallel so'rov bir vaqtda manfiy balansga tushirib yubora olmaydi.
5. **Sarflash tartibi:** avval muddati tez tugaydigan kreditlar (obuna), keyin sotib olingan paketlar.

### Generatsiya hayot sikli

```
 [UI: Generate]
      │
      ▼
 API: validatsiya → moderatsiya → narx hisoblash → credits.reserve()   (bitta tranzaksiya)
      │                                                   │
      │                                    balans yetmasa → 402 + "kredit sotib olish" oynasi
      ▼
 generation.status = queued → worker task ishga tushadi
      │
      ▼
 provider.submit() → status = processing (provider_job_id saqlanadi)
      │
      ├── webhook keldi (yoki polling, har 5-10 soniyada) ──┐
      │                                                     ▼
      │                                 natija → R2 ga ko'chirish → assets yozuvi
      │                                 → credits.capture() → status = succeeded
      │
      └── xato / 15 daqiqa timeout / moderatsiya rad etdi
                → zaxira modelga urinish (preset ruxsat bersa)
                → bo'lmasa credits.release() → status = failed + tushunarli xabar
```

### Interfeys

```ts
// packages/credits/src/index.ts
export interface Credits {
  /** Kreditni band qiladi. Balans yetmasa InsufficientCreditsError tashlaydi. */
  reserve(args: { walletId: string; amount: number; generationId: string }): Promise<void>;
  /** Band qilingan kreditni yakuniy yechadi (haqiqiy narx kamroq bo'lsa, farqi qaytariladi). */
  capture(args: { generationId: string; finalAmount?: number }): Promise<void>;
  /** Bandni bekor qiladi. Xato, timeout yoki bekor qilishda chaqiriladi. */
  release(args: { generationId: string; reason: string }): Promise<void>;
  grant(args: { walletId: string; amount: number; type: GrantType; expiresAt?: Date; idempotencyKey: string }): Promise<void>;
  balance(walletId: string): Promise<{ available: number; held: number }>;
}
```

**Majburiy testlar:** parallel `reserve` (race condition), ikki marta kelgan webhook, `capture`dan keyin `release` qilinmasligi, muddati o'tgan kreditlar, manfiy balans hech qachon bo'lmasligi.

## 2.6. Provayder adapterlari va model katalogi

```ts
// packages/providers/src/types.ts
export type Capability =
  | "text2video" | "image2video" | "firstLastFrame" | "references"
  | "extend" | "audio" | "lipsync" | "text2image" | "imageEdit" | "upscale";

export interface ModelSpec {
  id: string;                    // "veo-3.1-fast"
  provider: "google" | "fal" | "kling" | "byteplus" | "replicate";
  providerModel: string;         // provayder tomonidagi nomi
  kind: "video" | "image" | "audio";
  capabilities: Capability[];
  durations?: number[];          // [4, 6, 8]
  resolutions?: ("720p" | "1080p" | "4k")[];
  aspectRatios: ("16:9" | "9:16" | "1:1")[];
  maxReferenceImages?: number;
  pricing: PriceRule[];          // masalan { per: "second", usd: 0.10, when: { resolution: "720p" } }
}

export interface Provider {
  estimateCostUsd(model: ModelSpec, input: GenInput): number;
  submit(model: ModelSpec, input: GenInput, webhookUrl: string): Promise<{ providerJobId: string }>;
  getStatus(providerJobId: string): Promise<JobStatus>;           // polling uchun
  parseWebhook(req: Request): Promise<{ providerJobId: string; result: JobResult }>;
  cancel?(providerJobId: string): Promise<void>;
}
```

- **Model katalogi bazada turadi** (`models` jadvali). Admin paneldan narx, markup yoki holatni o'zgartirish mumkin, deploy qilish shart emas.
- **UI capability'lardan avtomatik quriladi.** Masalan, model `firstLastFrame`ni qo'llasa, "oxirgi kadr" maydoni chiqadi.
- **Bitta model bir nechta provayderda bo'lishi mumkin** (masalan, Kling fal orqali ham, to'g'ridan-to'g'ri ham). Router eng arzon yoki ishlab turganini tanlaydi.
- **Provayder natijasini darhol R2'ga ko'chiring.** Provayder URL'lari vaqtinchalik bo'ladi va bir necha soatda o'chib ketadi.

## 2.7. Xavfsizlik va ishonchlilik (minimal ro'yxat)

- [ ] API kalitlar faqat serverda, `NEXT_PUBLIC_` ichida hech qachon bo'lmasin
- [ ] Har bir webhook imzosi tekshiriladi va `webhook_events.external_id UNIQUE` orqali takrorlanmaydi
- [ ] Payme/Click so'rovlari IP va imzo bo'yicha tekshiriladi
- [ ] R2'dagi fayllar uchun signed URL; ommaviy galereya uchun faqat `is_public` assetlar
- [ ] Rate limit: foydalanuvchiga daqiqasiga N ta generatsiya, IP bo'yicha ro'yxatdan o'tish limiti
- [ ] Upload qilingan fayllar uchun MIME, o'lcham va davomiylik tekshiruvi, keyin moderatsiya
- [ ] Har bir pul operatsiyasi audit logga yoziladi (ledger o'zi audit log vazifasini bajaradi)
- [ ] Zaxira nusxa: Postgres PITR (Neon yoki Supabase'da bor)
- [ ] Status sahifa va provayder ishlamay qolganda zaxira modelga o'tish

## 2.8. Til va shriftlar

- Interfeys tillari: **o'zbek (lotin)**, **o'zbek (kirill)**, **rus**, keyinroq **ingliz**.
- O'zbek lotin alifbosida **oʻ, gʻ** harflari U+02BB belgisi bilan yoziladi. Odamlar ko'pincha `'` yoki `‘` ham yozadi, shuning uchun matnni normalizatsiya qiling.
- Motion uchun har bir shriftni **avtomatik test** qiling: oʻ/gʻ, kirill (ў, қ, ғ, ҳ) va rus harflari to'g'ri chiqyaptimi.
- Promptlar: foydalanuvchi o'zbekcha yozadi, LLM uni modelga mos inglizcha promptga aylantiradi (video modellar inglizchani eng yaxshi tushunadi).
