# 3. Platforma A: Hormoz Studio (Google Flow + Higgsfield uslubida)

## 3.1. Mahsulot g'oyasi

Foydalanuvchi o'zbek, rus yoki ingliz tilida g'oya yozadi yoki rasm yuklaydi va tayyor presetni tanlaydi. Natijada kino sifatidagi video yoki rasm oladi. Barcha yaxshi modellar bitta joyda, bitta kredit hamyoni bilan, so'mda to'lanadi.

**Uch asosiy foydalanish yo'li:**
1. **Preset orqali (eng tez):** Explore lentasidan effektni tanlaydi, rasm yuklaydi va "Yaratish"ni bosadi. Prompt yozish shart emas.
2. **Erkin yaratish:** prompt yozadi, model, format va davomiylikni tanlaydi. Prompt enhancer uni yaxshilaydi.
3. **Loyiha (scene builder):** bir nechta kadrni ketma-ket ulab, personajni saqlagan holda qisqa film yoki reklama yig'adi.

## 3.2. Ekranlar

| Ekran | Mazmuni |
|---|---|
| **Explore** | Preset va effektlar lentasi (Higgsfield uslubida), kategoriyalar: kamera, effekt, reklama, bayram, trend |
| **Create** | Prompt maydoni, enhancer tugmasi, model tanlash (kartalar: sifat, tezlik, narx), format (9:16, 16:9, 1:1), davomiylik, audio yoqish/o'chirish, referens rasmlar, aniq kredit narxi ko'rsatiladi |
| **Natija** | Video player, qayta yaratish, "extend", "shu kadrdan davom ettirish", Motion'ga yuborish, yuklab olish |
| **Projects** | Scene builder: kadrlar timeline'i, o'tishlar, musiqa, eksport (v2) |
| **Characters** | Personajlar kutubxonasi: rasmlar, tavsif, character sheet (v2) |
| **Library** | Barcha natijalar, papkalar, qidiruv |
| **Billing** | Balans, tariflar, Payme/Click/Uzum orqali to'lash, tarix |

## 3.3. Funksiyalar bosqichma-bosqich

### MVP (Faza 2, 3 hafta)
- [ ] Rasm: text-to-image va rasmni tahrirlash (2 ta model)
- [ ] Video: text-to-video va image-to-video (4 ta model, quyidagi jadvalga qarang)
- [ ] **Prompt enhancer** (o'zbek/rus tilidan inglizcha kino promptiga), natija tahrirlanadigan holda ko'rsatiladi
- [ ] **20 ta preset:** 12 ta kamera harakati va 8 ta effekt yoki uslub
- [ ] Generatsiya holati real vaqtda, xato bo'lsa kredit avtomatik qaytadi
- [ ] Library va yuklab olish (watermarksiz pullik, bepul tarifda kichik watermark)
- [ ] Kirish: Google, Telegram, telefon OTP
- [ ] To'lov: Payme va Click (kredit paketlari)
- [ ] Moderatsiya: prompt filtri va upload qilingan rasm filtri
- [ ] Admin: foydalanuvchilar, kreditni qo'lda berish, generatsiyalar logi, modellar katalogi

### v2 (Faza 4)
- [ ] **Projects / Scene builder:** timeline, extend, first/last frame o'tishlari, ffmpeg bilan yakuniy montaj va musiqa
- [ ] **Characters:** 3-5 ta rasm yuklanadi, tizim character sheet yasaydi (old, yon, 3/4 tomondan), keyin u referens qo'llaydigan modellarda ishlatiladi
- [ ] **Gapiruvchi avatar:** TTS (o'zbekcha ovoz) va lip-sync modeli
- [ ] Upscale (video va rasm)
- [ ] Ommaviy galereya va "shu promptni ishlatish" (remix), bu viral o'sish uchun
- [ ] Referral tizimi

### v3 (Faza 5)
- [ ] Jamoa (workspace) va umumiy kreditlar
- [ ] B2B API
- [ ] Telegram Mini App
- [ ] Har hafta yangi preset qo'shish (kontent-marketing konveyeri)

## 3.4. Modellar (2026-oktabr, narxlar taxminiy, tekshiring)

| Vazifa | Model | Qayerdan | Taxminiy tannarx | Izoh |
|---|---|---|---|---|
| Arzon/tez video | **Veo 3.1 Lite** | Gemini API | $0.05/s (720p), $0.08/s (1080p), audio bilan | 8 s ≈ $0.40. Preview holatida |
| Asosiy video | **Veo 3.1 Fast** | Gemini API | $0.10/s (720p), $0.12/s (1080p) | 8 s ≈ $0.80. Narx/sifat bo'yicha eng yaxshi |
| Premium video | **Veo 3.1 Standard** | Gemini API | $0.40/s (720p/1080p), $0.60/s (4K) | 8 s ≈ $3.20. Faqat Pro/Studio tarifda |
| Odam harakati, rasmdan video | **Kling 3.0** (Standard/Pro) | fal yoki Kling API | ~$0.084 dan $0.168/s gacha | Versiya va audioga bog'liq |
| Kinematik, multi-shot | **Seedance 2.0** | fal yoki BytePlus | fal'da ~$0.30/s (720p), BytePlus'da arzonroq | Trafik oshganda to'g'ridan-to'g'ri ulaning |
| Rasm | Nano Banana (Gemini image), Seedream, Flux | Gemini API / fal | taxminan $0.03 dan $0.15 gacha | Character sheet uchun Nano Banana kuchli |
| ❌ | **Sora 2** | | | **API 2026-yil 24-sentabrda yopilgan, rejaga kiritmang** |

**Provayder strategiyasi:**
- **Boshlanishda** fal.ai ishlatiladi: bitta API, bitta hisob, ko'p model. Veo esa to'g'ridan-to'g'ri Gemini API orqali ulanadi (u yerda arzonroq va Google faqat muvaffaqiyatli generatsiya uchun pul oladi).
- **Oyiga $2-3k xarajatdan oshganda** eng ko'p ishlatiladigan modellarni to'g'ridan-to'g'ri provayderdan oling (BytePlus, Kling), chunki fal ustiga qo'shimcha narx qo'yadi.
- Yangi model chiqqanda uni katalogga qo'shish 1 soatlik ish bo'lishi kerak (adapter allaqachon bor bo'ladi).

## 3.5. Kredit narxini hisoblash

```
kredit = ceil( tannarx_usd × markup / kredit_qiymati_usd )

kredit_qiymati_usd ≈ 0.08   (1 kredit ≈ 1 000 so'm, 1$ ≈ 12 500 so'm deb olingan, joriy kursni tekshiring)
markup             = 2.5    (video), 3.0 (rasm, chunki tannarxi kichik va infratuzilma ulushi kattaroq)
```

| Natija | Tannarx | Kredit | Narx (so'm) |
|---|---|---|---|
| Rasm (Nano Banana) | ~$0.04 | 2 | ~2 000 |
| Veo 3.1 Lite, 8 s, 720p | $0.40 | 13 | ~13 000 |
| Kling 3.0 Std, 5 s, audiosiz | ~$0.42 | 14 | ~14 000 |
| Veo 3.1 Fast, 8 s, 720p | $0.80 | 25 | ~25 000 |
| Seedance 2.0, 5 s, 720p (fal) | ~$1.50 | 47 | ~47 000 |
| Veo 3.1 Standard, 8 s, 1080p | $3.20 | 100 | ~100 000 |

Taqqoslash uchun: Higgsfield'da Veo 3.1 taxminan 40-70 kredit turadi, ya'ni $2.8 dan $4.9 gacha. Bizda Veo Fast taxminan $2 bo'ladi.

**Qoidalar:**
- Narx **"Yaratish" tugmasida** aniq ko'rsatiladi ("25 kredit"). Yashirin narx bo'lmaydi.
- Kurs va provayder narxi o'zgarsa, admin paneldan `markup` yoki `kredit_qiymati` yangilanadi.
- Xato yoki moderatsiya rad etsa, kredit **100% avtomatik qaytadi**.

## 3.6. Preset dvigateli

Preset bu kod emas, **ma'lumot**: model, prompt shabloni, parametrlar va namunaviy video. Yangi preset qo'shish 10 daqiqalik ish bo'lishi kerak.

```json
{
  "slug": "dolly-zoom-drama",
  "category": "camera",
  "title": { "uz": "Dramatik dolly-zoom", "ru": "Драматичный долли-зум", "en": "Dramatic dolly zoom" },
  "inputs": [{ "key": "image", "type": "image", "required": true }],
  "model": "kling-3.0-pro",
  "fallbackModels": ["veo-3.1-fast"],
  "promptTemplate": "{{subject}}. Dramatic dolly zoom (vertigo effect): the camera pulls back while zooming in, background stretches, cinematic lighting, shallow depth of field.",
  "subjectFrom": "auto-caption",
  "params": { "duration": 5, "aspectRatio": "9:16", "audio": false },
  "exampleAssetId": "ast_...",
  "tags": ["trend", "camera"]
}
```

- `subjectFrom: "auto-caption"`: foydalanuvchi faqat rasm yuklaydi, LLM rasmni tasvirlab `{{subject}}`ni o'zi to'ldiradi.
- **MVP uchun 20 ta preset:**
  - Kamera (12): dolly-in, dolly-out, orbit 360, crane up, drone fly-over, crash zoom, dolly-zoom, whip pan, FPV, handheld, slow push-in, bullet-time.
  - Effekt va uslub (8): "mahsulot aylanib turadi", "rasm jonlanadi", "kino trailer", "anime uslubi", "kecha va kunduz", "portlash bilan paydo bo'lish", "dron ko'rinishi", "eski kino plyonkasi".
- **Mahalliy presetlar** (v2): Registon ustidan uchish, milliy libosdagi portret, osh tayyorlash (kinematik), to'y raqsi, bayram salyutlari.

## 3.7. Prompt enhancer

**Model:** Claude (arzon va tez yo'l uchun `claude-haiku-5-5`, sifat kerak bo'lsa `claude-opus-5-5`. Kichik eval to'plamida solishtirib tanlang). Natija **structured outputs** (`output_config.format`, JSON Schema) orqali olinadi.

```
Kirish: "qizil choynakdan bug' chiqyapti, qishloq uyi, tong"
Chiqish (JSON):
{
  "prompt_en": "Close-up of a red ceramic teapot on a wooden table in a rustic Uzbek village house at dawn, gentle steam rising, warm golden light through a lattice window, slow push-in, shallow depth of field, cinematic, 35mm",
  "camera": "slow push-in",
  "suggested_model": "veo-3.1-fast",
  "aspect_ratio": "16:9",
  "safety": "ok"
}
```

- System prompt va kamera/uslub lug'ati **o'zgarmas prefiks** bo'ladi, shuning uchun prompt caching ishlaydi va narx keskin tushadi.
- Moderatsiya shu chaqiruvning o'zida bajariladi (`safety` maydoni). Qo'shimcha so'rov kerak emas.

## 3.8. Personajlar (consistency)

1. Foydalanuvchi 3-5 ta rasm yuklaydi va "bu men" yoki "menda ruxsat bor" belgisini qo'yadi (`consent_at`).
2. Rasm modeli (Nano Banana) **character sheet** yasaydi: old, yon va 3/4 tomondan, neytral fonda.
3. LLM personajning matnli tavsifini yozadi (soch, kiyim, yosh, uslub).
4. Generatsiyada referens rasmlarni qabul qiladigan modellarga (Veo "ingredients", Kling elements, Seedance references) shu rasmlar, qolganlariga esa tavsif uzatiladi.

> ⚠️ **Huquqiy eslatma:** yuz rasmlari biometrik ma'lumot hisoblanishi mumkin. 2026-yil martidagi o'zgartirishlardan keyin ham O'zbekistonda **biometrik ma'lumotlar mamlakat ichidagi serverlarda** saqlanishi kerak. Characters funksiyasini ishga tushirishdan oldin yurist bilan maslahatlashing (6-hujjatga qarang). Mashhur shaxslar va voyaga yetmaganlar rasmlari taqiqlanadi.

## 3.9. Scene builder (Flow uslubida, v2)

- **Loyiha** bu kadrlar ro'yxati. Har bir kadr bitta generatsiya bo'lib, prompti, modeli va natijasi saqlanadi.
- **Extend:** oldingi kadrning oxirgi freymi (ffmpeg bilan ajratiladi) keyingi kadr uchun image-to-video kirishi bo'ladi.
- **O'tish:** ikki kadr orasida first/last frame modeli bilan silliq o'tish yaratiladi.
- **Eksport:** worker ffmpeg bilan kadrlarni ulaydi (crossfade), musiqa va subtitr qo'shadi, keyin R2'ga yuklaydi.
- Loyiha holati `projects.data` (jsonb) ichida saqlanadi. Motion bilan bir xil yondashuv.

## 3.10. Bepul tarif va suiiste'molga qarshi choralar

Video generatsiya qimmat, shuning uchun bepul tarif eng katta xavf manbai.

- Ro'yxatdan o'tganda **10 kredit** beriladi: bir nechta rasm yoki **1 ta** Veo Lite video uchun yetadi.
- **Telefon raqami tasdiqlanmaguncha** video yaratib bo'lmaydi. Bitta raqam bitta akkauntga.
- Qurilma fingerprinti va IP bo'yicha limit qo'yiladi. Vaqtinchalik email domenlari bloklanadi.
- Bepul natijada watermark bo'ladi, navbatda past ustuvorlik beriladi.

## 3.11. Moderatsiya siyosati

- Taqiqlanadi: NSFW, zo'ravonlik, real shaxslar va mashhurlarning deepfake'lari, voyaga yetmaganlar, siyosiy dezinformatsiya, boshqalarning brendi yoki logotipini noqonuniy ishlatish.
- Uch qatlam bor: (1) LLM prompt filtri, (2) upload qilingan rasm moderatsiyasi, (3) provayderning o'z filtri. Provayder rad etsa, foydalanuvchiga tushunarli xabar ko'rsatiladi va kredit qaytariladi.
- Barcha generatsiyalar log qilinadi. Shikoyat tugmasi va admin paneldan bloklash imkoniyati bo'ladi.
- Veo natijalarida Google'ning SynthID watermarki bor. Foydalanish shartlarida "AI bilan yaratilgan" ekanini ochiq ko'rsatish talabini yozing.

## 3.12. MVP "tayyor" mezonlari

- [ ] Yangi foydalanuvchi ro'yxatdan o'tib, **3 daqiqa ichida** birinchi rasmini, 5 daqiqa ichida birinchi videosini oladi
- [ ] 4 ta video modeli, 2 ta rasm modeli va 20 ta preset ishlaydi
- [ ] Payme va Click orqali to'lov, kredit avtomatik tushadi (sandbox va real holatda sinalgan)
- [ ] Xato bo'lgan generatsiyaning **100%** kreditlari avtomatik qaytadi (test bilan isbotlangan)
- [ ] Generatsiya muvaffaqiyati ≥ 95%, xatolar Sentry'da, LLM so'rovlari Langfuse'da ko'rinadi
- [ ] Mobil (telefon) brauzerda to'liq ishlaydi, chunki trafikning ko'pi Instagram va Telegramdan keladi
- [ ] Interfeys o'zbek va rus tillarida

## Manbalar

- Veo 3.1 narxlari: [CloudZero](https://www.cloudzero.com/blog/veo-3-pricing/), [benchlm.ai](https://benchlm.ai/media-pricing/veo), [WaveSpeed](https://wavespeed.ai/blog/cost-and-billing/google-veo-3-pricing/)
- fal.ai va boshqa API narxlari: [fal: text-to-video APIs](https://fal.ai/learn/tools/best-text-to-video-apis-2026), [fal: image-to-video APIs](https://fal.ai/learn/tools/best-image-to-video-apis-2026), [DevTk.AI](https://devtk.ai/en/blog/ai-video-generation-pricing-2026/), [teamday.ai](https://www.teamday.ai/blog/ai-image-video-api-providers-comparison-2026), [buildmvpfast](https://www.buildmvpfast.com/api-costs/ai-video)
- Higgsfield kredit narxlari: [Hackceleration](https://hackceleration.com/labs/review/higgsfield), [layer3labs](https://www.layer3labs.io/guides/higgsfield-ai-pricing)
