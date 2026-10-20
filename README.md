# Hormoz: 2 ta AI video platforma, to'liq tahlil va qurish rejasi

> Holat: **2026-yil oktabr**. Model narxlari va API'lar har oy o'zgaradi, shuning uchun bu hujjatlardagi barcha
> raqamlarni **taxminiy** deb qabul qiling va ishga tushirishdan oldin provayderning rasmiy sahifasidan tekshiring.
> Kodda narxni hech qachon qattiq yozib qo'ymang (hardcode qilmang), uni bazada saqlang.

## Ikki mahsulot, bitta yadro

| | **Platforma A: Hormoz Studio** | **Platforma B: Hormoz Motion** |
|---|---|---|
| Nimaga o'xshaydi | Google Flow + Higgsfield | Claude Motion / Canva animatsiya, lekin tezroq va lokal |
| Nima qiladi | Matn yoki rasmdan AI video va rasm yaratadi. Kamera presetlari, sahna yig'uvchi (scene builder), doimiy personajlar | Prompt, skript, sayt yoki jadvaldan tayyor **motion-grafika** video yasaydi: kinetik tipografiya, infografika, reklama |
| Texnik mohiyat | Tashqi video modellarni (Veo, Kling, Seedance...) boshqaruvchi orkestrator | LLM tayyor komponentlar kutubxonasi ustida **JSON sahna grafi** yozadi, keyin Remotion uni render qiladi |
| Tezlik maqsadi | Modelga bog'liq (odatda 30 soniyadan 3 daqiqagacha) | **Preview ≤ 60 soniya, tayyor MP4 ≤ 2 daqiqa** (5 daqiqalik maqsaddan ancha tez) |
| Tannarx (1 natija) | Yuqori ($0.40 dan $3+ gacha) | Past (taxminan $0.10 dan $0.25 gacha) |
| Yalpi marja | ~55-60% | ~75-85% |

## Asosiy qarorlar (TL;DR)

1. **Model o'qitmaymiz.** Biz orkestrator/agregatormiz. Qiymatimiz UX, presetlar, o'zbek va rus tili, mahalliy to'lov (Payme/Click/Uzum), so'mdagi narx va Telegram integratsiyasidan iborat.
2. **Ikki mahsulotning yadrosi bitta:** bitta akkaunt, bitta kredit hamyoni, bitta navbat (queue), bitta storage. Hammasi bitta monorepo ichida. Domenlar va landinglar alohida bo'ladi (pozitsiyalash va SEO uchun).
3. **Motion tezligining siri:** LLM erkin kod yozmaydi. U oldindan yozilib, sinovdan o'tgan komponentlar kutubxonasi ustida qisqa JSON yozadi. Preview brauzerda darhol chiqadi (Remotion Player), render esa parallel bo'laklarda bajariladi.
4. **Ketma-ketlik:** Yadro (2 hafta), keyin Studio MVP (3 hafta), keyin Motion MVP (5 hafta), keyin integratsiya, keyin Telegram bot va API. Jami **~16 hafta**.
5. **Pul bilan ishlaydigan kod** (kredit ledger, webhook, to'lov) eng ehtiyotkorlik bilan, testlar bilan yoziladi. UI'ni bemalol vibe-code qilaverishingiz mumkin.
6. **Bitta provayderga bog'lanmang.** Sora API 2026-yil 24-sentabrda yopildi. Har bir model adapter orqali ulanadi va zaxira (fallback) modelga ega bo'ladi.

## Hujjatlar

| # | Fayl | Mazmuni |
|---|---|---|
| 1 | [docs/01-bozor-tahlili.md](docs/01-bozor-tahlili.md) | Google Flow, Higgsfield, Claude Motion va boshqalar; bo'shliqlar; auditoriya; pozitsiyalash |
| 2 | [docs/02-umumiy-yadro.md](docs/02-umumiy-yadro.md) | Arxitektura, tech stack, monorepo, baza sxemasi, kredit tizimi, provayder adapterlari, xavfsizlik |
| 3 | [docs/03-platforma-A-studio.md](docs/03-platforma-A-studio.md) | Studio: funksiyalar, modellar, presetlar, personajlar, scene builder, narxlash, MVP mezonlari |
| 4 | [docs/04-platforma-B-motion.md](docs/04-platforma-B-motion.md) | Motion: tezlik arxitekturasi, sahna DSL, komponentlar kutubxonasi, LLM pipeline, render, sifat nazorati |
| 5 | [docs/05-yol-xaritasi.md](docs/05-yol-xaritasi.md) | 16 haftalik reja: haftama-hafta vazifalar, "tayyor" mezonlari, KPI, launch rejasi |
| 6 | [docs/06-biznes-moliya-risklar.md](docs/06-biznes-moliya-risklar.md) | Taklif (offer), tariflar, unit-ekonomika, boshlang'ich byudjet, huquqiy masalalar, risklar, marketing |

## Birinchi 3 kunda nima qilish kerak

1. **Yuridik shaxs va to'lov tizimlari:** MChJ ochish va Payme, Click, Uzum merchant arizalarini topshirish. Ular eng uzoq kutiladigan qism, shuning uchun hozirdan boshlang.
2. **Akkauntlar:** Anthropic API, Google AI Studio/Vertex (Veo), fal.ai, Cloudflare (R2), AWS (Remotion Lambda), Vercel, Sentry, PostHog.
3. **Raqobatchilarni sinash:** Higgsfield, Google Flow va Claude Motion'da kamida bir kun ishlab ko'ring va kuzatuvlaringizni yozib boring.
