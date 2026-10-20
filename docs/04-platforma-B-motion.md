# 4. Platforma B: Hormoz Motion (2 daqiqada motion-grafika video)

## 4.1. Maqsad

> **Prompt yuboriladi, 60 soniya ichida brauzerda jonli preview chiqadi, 2 daqiqa ichida tayyor 1080p MP4 olinadi.**
> 30 soniyalik video uchun. "5 daqiqa" va'dasidan ancha tez, shuning uchun marketingda "2 daqiqa" deb ayting.

Natijalar: kinetik tipografiya, reklama roliklari, infografika, mahsulot videolari, e'lonlar, tabriklar, Telegram post'idan video. Formatlar: 9:16 (Reels, TikTok, Shorts), 1:1, 16:9.

## 4.2. Nega Claude Code bilan sekin, bizda esa tez

| | Claude Code + Remotion (qo'lda) | Hormoz Motion |
|---|---|---|
| Kod | Har safar noldan TSX yoziladi (minglab token) | **Kod yozilmaydi.** LLM tayyor komponentlar uchun ~1-3 KB JSON yozadi |
| Xatolar | Kompilyatsiya va tip xatolari, qayta urinishlar | JSON sxema bo'yicha tekshiriladi va 1 marta avtomatik tuzatiladi |
| Dizayn sifati | Har safar har xil, ko'pincha "dasturchi dizayni" | Dizayner darajasida oldindan sinalgan komponentlar va mavzular |
| Preview | Render tugashini kutish kerak | **Darhol**: Remotion Player brauzerda JSON'dan o'ynatadi |
| Render | Bitta noutbukda ketma-ket | Bulutda **parallel bo'laklar** (Lambda) |
| Tahrir | "Raqamni o'zgartir" degani yana kod yozish demak | JSON Patch: 1-3 soniya, preview darhol yangilanadi |
| Kirish | Terminal, Node, dasturlash bilimi kerak | Brauzer yoki Telegram, o'zbek tilida bitta gap |

**Asosiy g'oya:** Claude Code'dagi qimmat va sekin ish (komponent yozish) **bir marta**, oldindan, biz tomondan qilinadi. Foydalanuvchi so'rovida esa faqat tez va arzon qism (tanlash va to'ldirish) bajariladi.

## 4.3. Pipeline

```
 1. KIRISH       prompt | skript | sayt URL | CSV/Google Sheets | mahsulot rasmi | Telegram post
                 + brend to'plami (logo, ranglar, shrift) + format + davomiylik + til + ovoz
        │
        ▼
 2. DIRECTOR (LLM, structured output, stream)                         ~5-15 s
    ssenariy → storyboard: sahnalar ro'yxati (turi, davomiyligi, matn, rekvizitlar)
    sahnalar oqim bilan kelgan sari UI'da birma-bir paydo bo'ladi
        │
        ▼
 3. COMPOSER (deterministik kod, LLM emas)                            <1 s
    zod validatsiya → matnni sig'dirish (fitText) → ranglar kontrasti → vaqtlarni hisoblash
    xato bo'lsa xatolar ro'yxati LLM'ga qaytariladi (1 marta tuzatish)
        │
        ├──────────────── parallel ────────────────┐
        ▼                       ▼                  ▼
 4a. TTS (har sahna)     4b. Ikonalar/stok      4c. Musiqa tanlash
     so'z vaqtlari bilan     (Iconify, Pexels)      (beat markerlari)
     ~3-8 s                  ~2-5 s                 <1 s
        └──────────────────────┬───────────────────┘
                               ▼
 5. PREVIEW: Remotion Player brauzerda JSON'ni darhol o'ynatadi        0 s render
                               │
 6. TAHRIR:  forma (zod'dan avtomatik) | chat ("3-sahnani qisqartir") → JSON Patch
                               │
 7. RENDER:  Remotion Lambda (parallel bo'laklar) → R2 → yuklab olish yoki ulashish   ~20-60 s
```

### Tezlik byudjeti (30 s video, 9:16, 1080p)

| Qadam | Maqsad (p50) | Qanday erishiladi |
|---|---|---|
| Director | ≤ 15 s | Komponent katalogi va system prompt keshda turadi (prompt caching), stream, past `effort` |
| Composer va assetlar | ≤ 10 s | Hammasi parallel, ikonalar va musiqa lokal keshda |
| **Birinchi preview** | **≤ 30-45 s** | Player brauzerda ishlaydi, server render kerak emas |
| Render | ≤ 60 s | Lambda: 900 kadr ~30 ta parallel funksiyaga bo'linadi |
| **Tayyor MP4** | **≤ 2 daqiqa** | |

## 4.4. Sahna DSL (JSON) namunasi

```json
{
  "version": 1,
  "format": "9:16",
  "fps": 30,
  "theme": "bold-dark",
  "brand": { "primary": "#7C3AED", "accent": "#22C55E", "font": "Montserrat", "logo": "asset://ast_logo" },
  "audio": {
    "music": { "track": "upbeat-03", "volume": 0.25, "syncToBeats": true },
    "voice": { "provider": "azure", "voice": "uz-UZ-MadinaNeural" }
  },
  "scenes": [
    {
      "type": "KineticTitle",
      "duration": 2.5,
      "voiceover": "Biznesingizni onlayn olib chiqing!",
      "props": { "text": "Biznesingizni onlayn olib chiqing", "highlight": ["onlayn"] },
      "transition": { "type": "slide-up", "duration": 0.4 }
    },
    {
      "type": "StatCounter",
      "duration": 3,
      "voiceover": "Xaridorlarning 87 foizi avval internetda qidiradi.",
      "props": { "value": 87, "suffix": "%", "label": "xaridorlar avval internetda qidiradi" }
    },
    {
      "type": "FeatureList",
      "duration": 4,
      "props": {
        "title": "Biz nima qilamiz",
        "items": [
          { "icon": "lucide:globe", "text": "Sayt 7 kunda" },
          { "icon": "lucide:send", "text": "Telegram bot" },
          { "icon": "lucide:bar-chart-3", "text": "Reklama va analitika" }
        ]
      }
    },
    {
      "type": "CTAEndCard",
      "duration": 3,
      "voiceover": "Bugunoq yozing!",
      "props": { "title": "Bugunoq boshlang", "button": "t.me/brend", "showLogo": true }
    }
  ]
}
```

**Nega JSON?** Kichik (tez generatsiya qilinadi), tekshirib bo'ladi (zod), tahrirlash oson (JSON Patch), versiyalash oson, **xavfsiz** (serverda begona kod ishlamaydi) va bir xil JSON doim bir xil video beradi.

## 4.5. Komponentlar kutubxonasi (Motion'ning asosiy boyligi)

Har bir komponent **shartnoma** asosida yoziladi: sxema, tavsif (LLM uchun), standart davomiylik va React komponentning o'zi.

```tsx
// packages/motion-engine/src/scenes/StatCounter.tsx
export const StatCounter = defineScene({
  type: "StatCounter",
  description:
    "Katta raqam 0 dan berilgan qiymatgacha sanaladi, ostida qisqa izoh chiqadi. Statistika, foiz va natijalar uchun.",
  schema: z.object({
    value: z.number(),
    prefix: z.string().max(4).optional(),
    suffix: z.string().max(4).optional(),
    label: z.string().max(60),
  }),
  defaultDuration: 3,
  minDuration: 2,
  maxDuration: 6,
  formats: ["9:16", "1:1", "16:9"],
  component: StatCounterView, // useCurrentFrame + spring/interpolate
});
```

- Katalogdan **avtomatik** ravishda LLM uchun JSON Schema va tavsiflar ro'yxati quriladi (`zod-to-json-schema`). Komponent qo'shsangiz, LLM uni darhol "ko'radi".
- Tahrir paneli ham shu sxemadan avtomatik quriladi.

### MVP uchun 25 ta komponent (v2'da 50+)

| Guruh | Komponentlar |
|---|---|
| Tipografiya (7) | KineticTitle, WordByWord, HighlightSentence, BigNumber, QuoteCard, TypewriterText, SplitTitle |
| Biznes va infografika (7) | StatCounter, BarChart, LineChart, DonutChart, ComparisonSplit, Timeline, ProcessSteps |
| Reklama va mahsulot (7) | ProductHero (rasm va narx), DiscountBurst, FeatureList, PhoneMockup, TestimonialCard, LogoReveal, CTAEndCard |
| Ijtimoiy (4) | LowerThird, KaraokeSubtitles, SocialHandleCard, QRCodeCard |
| Fon va o'tishlar | 6 ta fon (gradient, zarrachalar, setka, noise, blob, naqsh) va 6 ta o'tish (slide, wipe, zoom, blur, mask-reveal, glitch) |

### Mavzular (theme), MVP uchun 6 ta
Minimal Light, Bold Dark, Neon, Corporate Blue, Luxury Gold va **Milliy naqsh** (o'zbek ornamenti bilan, hech bir raqobatchida yo'q).

Mavzu bu dizayn tokenlari to'plami: ranglar, shriftlar, burchaklar, soyalar, animatsiya "xarakteri" (spring sozlamalari). Komponentlar ranglarni qattiq yozmaydi, faqat tokenlardan oladi.

### Mahalliy shablonlar (tayyor ssenariylar, v2)
To'y taklifnomasi, tug'ilgan kun, Navro'z, hayit tabriklari, Uzum Market mahsulot kartochkasi, ko'chmas mulk, o'quv markazi qabuli, restoran aksiyasi, Telegram post'idan video.

## 4.6. Director: LLM qismi

**Model:** sukut bo'yicha `claude-opus-5-5` (`effort: "low"` yoki `"medium"`). Tez tahrirlar uchun `claude-haiku-5-5` yoki `claude-sonnet-5-5`ni sinab ko'ring. Tanlovni **eval to'plami** natijasiga qarab qiling (4.9-bo'lim).

**Texnik qoidalar (Claude API, 2026):**
- JSON **structured outputs** orqali olinadi (`output_config: { format: { type: "json_schema", ... } }`). Yangi modellarda majburiy `tool_choice` (`any`/`tool`) **400 xato beradi**, shuning uchun ishlatmang.
- **Prompt caching:** system prompt, komponent katalogi va mavzular o'zgarmas prefiks bo'ladi. Ularga sana, foydalanuvchi ID'si kabi o'zgaruvchan narsani qo'shmang, aks holda kesh buziladi. `usage.cache_read_input_tokens` bilan tekshiring.
- **Stream** ishlating: sahnalar kelgan sari UI'da paydo bo'ladi, bu foydalanuvchi uchun tezlik hissini 2 baravar oshiradi.
- Ikki bosqichli yondashuv yaxshiroq ishlaydi: (1) ssenariy va sahnalar rejasi, (2) har sahnaning rekvizitlari. Lekin MVP'da bitta chaqiruv yetadi.

**Director system prompt'ining tuzilishi (qisqacha):**
```
Sen motion-dizayn direktorisan. Vazifang: foydalanuvchi so'rovidan qisqa, ta'sirli video ssenariysi tuzish.
- Faqat quyidagi katalogdagi sahna turlarini ishlat: {KATALOG}
- Har sahnada bitta g'oya. Matnlar qisqa: sarlavha ≤ 6 so'z, izoh ≤ 12 so'z.
- Birinchi 2 soniyada "hook" bo'lsin. Oxirida CTA bo'lsin.
- Jami davomiylik {DURATION} soniya (±1).
- Til: {LANG}. O'zbek lotin yozuvida oʻ va gʻ harflarini to'g'ri yoz.
- Brend ranglari va logotipni ishlat, o'zingdan rang o'ylab topma.
```

**Tahrir chat orqali (JSON Patch, RFC 6902):**
```
Foydalanuvchi: "2-sahnadagi raqamni 45% qil va urg'u rangini yashil qil"
LLM javobi:
[
  { "op": "replace", "path": "/scenes/1/props/value", "value": 45 },
  { "op": "replace", "path": "/brand/accent", "value": "#22C55E" }
]
```
Patch kichik bo'lgani uchun javob 1-3 soniyada keladi, preview esa darhol yangilanadi. Har bir patch tarixga yoziladi, shuning uchun "Ortga qaytarish" (undo) bepul.

**Taxminiy LLM narxi (bitta 30 s video):**
- Opus 5.5 ($4 / $20 har 1M token, kesh o'qish $0.20/1M): ~15k keshlangan kirish ≈ $0.003, ~2k yangi kirish ≈ $0.008, ~3-5k chiqish (fikrlash bilan) ≈ $0.06-0.10. **Jami ≈ $0.07-0.11.**
- Haiku 5.5 ($0.10 / $0.50): bir sentdan ham kam.

## 4.7. Ovoz, subtitr va musiqa

- **O'zbekcha TTS variantlari** (sifatini o'zingiz sinab ko'ring):
  - Azure Neural TTS'da `uz-UZ` ovozlari bor (masalan, Madina va Sardor). So'z chegaralari (word boundary) vaqtlarini qaytaradi va arzon.
  - ElevenLabs: rus va ingliz tilida juda kuchli. O'zbek tilini qanchalik yaxshi qo'llashini tekshiring. Belgilar bo'yicha timestamp qaytaradi.
  - Mahalliy o'zbek TTS startaplari: sifatini va API'sini tekshiring.
- **Sahna vaqti ovozga moslashadi:** TTS davomiyligi sahnaning `duration` qiymatini avtomatik o'zgartiradi (min va max chegarasida).
- **Karaoke subtitrlar:** so'z vaqtlari `KaraokeSubtitles` komponentiga beriladi. Reels uchun bu juda muhim, chunki ko'pchilik ovozsiz ko'radi.
- **Musiqa:** tijoriy litsenziyali kutubxona sotib oling (Pixabay kabi bepul manbalarning litsenziyasini tekshiring). Har trek uchun BPM va beat markerlarini **oldindan** hisoblab qo'ying (librosa). `syncToBeats: true` bo'lsa, sahna almashuvlari beat'ga to'g'rilanadi. Bu videoni "professional" his qildiradi.

## 4.8. Render

| Variant | Afzallik | Kamchilik | Qachon |
|---|---|---|---|
| **Remotion Lambda** (AWS) | Parallel bo'laklar, 30 s video ~20-60 s, serverni boshqarish shart emas | AWS sozlash kerak | **MVP va undan keyin (tavsiya)** |
| O'z workerlarimiz (Hetzner, `renderMedia`) | Bashorat qilinadigan oylik narx | Sekinroq, scaling o'zingizda | Trafik katta va bir tekis bo'lsa |
| HyperFrames (HeyGen, Apache 2.0) | HTML asosida, litsenziya bepul, agentlar uchun qurilgan | Yosh loyiha, Player ekvivalenti zaifroq | Zaxira variant yoki "custom code" rejimi uchun |

**Remotion litsenziyasi:** jismoniy shaxslar va **3 kishigacha** kompaniyalar uchun bepul. Kattaroq kompaniyalar uchun "Automators" litsenziyasi: **$0.01/render, oyiga kamida $100** (prompt-to-video ilovalari aynan shunga kiradi). Rasmiy sahifadan tasdiqlang. Narxga allaqachon kiritilgan.

**Muhandislik qoidalari:**
- `packages/motion-engine` render qatlamidan ajratilgan bo'lsin: DSL'dan kompozitsiyaga o'tish bitta joyda bo'ladi. Shunda kerak bo'lsa HyperFrames'ga o'tish mumkin.
- Shriftlar, ikonalar va musiqa Lambda bundle'iga yoki R2'ga oldindan joylanadi. Render paytida tashqi tarmoqqa chiqmaslik tezlik va barqarorlik beradi.
- Har render natijasi R2'da `hash(DSL + engine_version)` kaliti bilan keshlanadi. Bir xil so'rov qayta render qilinmaydi.

## 4.9. Sifat nazorati

1. **Sxema:** zod validatsiya. Xato bo'lsa xatolar matni LLM'ga qaytariladi va 1 marta qayta so'raladi. Yana xato bo'lsa, eng yaqin xavfsiz qiymat qo'yiladi.
2. **Matn sig'ishi:** `@remotion/layout-utils` (`measureText`, `fitText`) bilan matn sig'masa shrift kichraytiriladi. Juda uzun bo'lsa LLM'dan qisqartirish so'raladi.
3. **Kontrast:** matn va fon o'rtasidagi kontrast WCAG bo'yicha kamida 4.5:1 bo'lishi kerak. Bo'lmasa, mavzudagi muqobil rang olinadi.
4. **Xavfsiz zonalar:** 9:16 formatda yuqori va pastki ~15% TikTok/Reels interfeysi ostida qoladi, matn u yerga qo'yilmaydi.
5. **Davomiylik:** sahnalar yig'indisi so'ralgan davomiylikka teng bo'lishi kerak (±1 s).
6. **Glif testi:** oʻ, gʻ va kirill harflari tanlangan shriftda borligi tekshiriladi.
7. **Vizual QA (premium, v2):** 3 ta kalit kadr still sifatida render qilinadi va vision model "ustma-ust tushgan elementlar, o'qilmaydigan matn bormi?" deb baholaydi.
8. **Eval to'plami:** 50-100 ta haqiqiy so'rov (o'zbek, rus, ingliz). Har prompt yoki model o'zgarishida avtomatik ishga tushiriladi va quyidagilar o'lchanadi: sxema to'g'riligi, matn sig'ishi, davomiylik, kechikish va narx. Bu "his bo'yicha" emas, raqam bo'yicha qaror qilish imkonini beradi.

## 4.10. Kirish rejimlari

| Rejim | Qanday ishlaydi | Bosqich |
|---|---|---|
| Prompt orqali video | Bitta gap yoziladi va Director hammasini o'zi qiladi | MVP |
| Skript orqali video | Foydalanuvchi matnni o'zi beradi, Director uni sahnalarga bo'ladi | MVP |
| Telegram post orqali video | Post yuboriladi (matn va rasm), natijada 9:16 video olinadi | MVP+ (bot bilan) |
| Sayt URL orqali promo | Playwright logo, ranglar, sarlavhalar va skrinshotlarni yig'adi, keyin brend to'plami avtomatik yaratiladi | v2 |
| CSV yoki Sheets orqali infografika | Jadvaldan diagramma sahnalari quriladi | v2 |
| Mahsulot rasmidan reklama | Fon olib tashlanadi, ProductHero, narx va CTA qo'shiladi | v2 |
| Ommaviy rejim (API) | 1 000 qatorli CSV'dan 1 000 ta shaxsiylashtirilgan video (ko'chmas mulk, e-commerce) | v3, B2B |
| Studio B-roll | Studio'da yaratilgan AI klip sahna foni sifatida ishlatiladi | v2 (integratsiya) |

## 4.11. "Custom code" rejimi (v2, premium)

Katalogda yo'q noyob g'oya uchun (masalan, "logotipim zarrachalardan yig'ilsin"):
1. LLM komponent shartnomasi bo'yicha **yangi TSX komponent** yozadi (bu yerda Claude Code yondashuvi ishlatiladi, lekin cheklovlar bilan).
2. Kod **izolyatsiyalangan** worker'da esbuild bilan kompilyatsiya qilinadi. Tarmoq va fayl tizimiga kirish yo'q, vaqt va xotira limiti qo'yiladi.
3. 3 ta test kadr render qilinadi. Xato bo'lsa, xato matni LLM'ga qaytariladi (maksimum 3 urinish).
4. Muvaffaqiyatli komponent foydalanuvchining shaxsiy kutubxonasiga saqlanadi va keyin oddiy JSON orqali qayta ishlatiladi.

Bu rejim sekinroq (1-3 daqiqa) va qimmatroq, shuning uchun alohida kredit narxiga ega.

> ⚠️ Foydalanuvchi uchun yozilgan kod serverdagi headless Chrome ichida ishlaydi. Uni **hech qachon** asosiy render workerlarida ishga tushirmang, faqat alohida sandbox'da.

## 4.12. Unit-ekonomika (bitta 30 s video)

| Xarajat | Taxminan |
|---|---|
| LLM (Director va 2-3 ta tahrir) | $0.05-0.12 |
| TTS (~400-500 belgi) | $0.01-0.05 |
| Render (Lambda) | bir necha sent |
| Remotion litsenziyasi (agar kerak bo'lsa) | $0.01 |
| Storage va CDN (R2) | ~0 |
| **Jami** | **≈ $0.10-0.25** |

Narx: **10 kredit (~10 000 so'm, ~$0.80)** bitta 30 s video uchun, tahrirlar va preview bepul. Yalpi marja **~70-85%**.

## 4.13. MVP "tayyor" mezonlari

- [ ] O'zbekcha bitta gapdan 30 s, 9:16 video. Preview p50 ≤ 45 s, MP4 p50 ≤ 2 daqiqa
- [ ] 25 ta komponent, 6 ta mavzu, 3 ta format
- [ ] O'zbek, rus va ingliz tilida ovoz va karaoke subtitrlar
- [ ] Chat orqali tahrir (JSON Patch) va forma orqali tahrir, undo ishlaydi
- [ ] Eval to'plamida sxema to'g'riligi ≥ 99%, matn sig'masligi ≤ 2%
- [ ] Brend to'plami: logo, 2 ta rang va shrift saqlanadi va har videoda qo'llanadi
- [ ] Studio bilan umumiy kredit hamyoni

## Manbalar

- Claude Code va Remotion bilan motion-grafika: [rescroll.ai](https://www.rescroll.ai/blog/claude-code-remotion-motion-graphics-2026), [MindStudio](https://www.mindstudio.ai/blog/remotion-claude-code-animated-logo-reveals-motion-graphics), [Medium](https://medium.com/@tahirbalarabe2/build-ai-motion-graphics-with-claude-and-remotion-0990e6bbf773), [Pexo](https://pexo.ai/blog/can-claude-code-make-videos-5767)
- Remotion litsenziyasi: [remotion.dev/docs/license/pricing](https://www.remotion.dev/docs/license/pricing), [Remotion company licenses](https://www.remotion.dev/blog/company-licenses)
- HyperFrames: [GitHub](https://github.com/heygen-com/hyperframes), [themenonlab](https://themenonlab.blog/blog/heygen-hyperframes-video-as-code)
- Claude Motion: [Anthropic yordam markazi](https://support.claude.com/en/articles/17454997)
