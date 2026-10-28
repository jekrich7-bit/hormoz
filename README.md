# Hormoz — ingliz tili mock va tayyorlov platformasi (ishchi nom)

IELTS uslubidagi kompyuter mock imtihoni, darajani aniqlash (placement), darajama-daraja o'qitish va guruhlash platformasi. Platforma Band 5.0 dan 9.0 gacha tayyorlaydi va O'zbekiston bozori uchun mo'ljallangan.

**Holat:** reja bosqichi. Kod hali yozilmagan. Qurish Claude Code bilan `docs/05-yol-xaritasi.md` bo'yicha boshlanadi.

> Mustaqil tayyorlov loyihasi. IELTS — British Council, IDP: IELTS Australia va Cambridge University Press & Assessment'ning ro'yxatdan o'tgan tovar belgisi; loyiha ular bilan aloqador emas.

## Hujjatlar

| # | Hujjat | Nima haqida |
|---|---|---|
| 01 | [Bozor va ilmiy dalillar](docs/01-bozor-va-ilmiy-dalillar.md) | O'zbekiston bozori, raqobatchilar, 7.0 uchun xarajatni qaytarish, o'qitish fanidagi dalillar, AI baholash ishonchliligi, huquqiy cheklovlar |
| 02 | [Daraja, placement, guruhlash va dastur](docs/02-daraja-placement-guruhlash-dastur.md) | IELTS formati, xom ball → band, L0–L6 darajalar, adaptiv placement, guruhlash, Band 5/6/7/8/9 dasturlari, Writing/Speaking baholash usuli |
| 03 | [Texnik arxitektura va Claude Code](docs/03-texnik-arxitektura-va-claude-code.md) | Eng kerakli vositalar, arxitektura, ma'lumotlar modeli, AI narxlari, STT tanlash testi, MCP/skill/subagent/hook, `CLAUDE.md` namunasi |
| 04 | [Biznes va moliya](docs/04-biznes-va-moliya.md) | Narxlar zinapoyasi, birlik iqtisodiyoti, xarajatlar, zararsizlik nuqtasi, ssenariylar, marketing, KPI, xavflar |
| 05 | [Yo'l xaritasi](docs/05-yol-xaritasi.md) | Sprint 0–12 (~6 oy), asosiy bosqichlar, kodsiz ishlar, ochiq savollar |
| 06 | [Tashqi reja tahlili](docs/06-tashqi-reja-tahlili.md) | Boshqa manbadan olingan rejaning har bandiga baho |
| 07 | [Tavsiyalar va bo'limlar](docs/07-tavsiyalar-va-bolimlar.md) | Asosiy tavsiyalar, nimani qurmasligimiz, rollar, sayt xaritasi, har bir bo'lim tavsifi |
| 08 | [Dizayn tizimi](docs/08-dizayn-tizimi.md) | Tamoyillar, brend, ranglar, shriftlar, komponentlar, asosiy ekranlarning sxemalari |
| 09 | [Admin panel](docs/09-admin-panel.md) | Ruxsatlar matritsasi, 20 ta modul, kontent workflow'i, sprintlar bo'yicha reja |
| 10 | [Skelet va texnik spetsifikatsiya](docs/10-skelet-va-texnik-spetsifikatsiya.md) | Versiyalar, papkalar, marshrutlar, ma'lumotlar modeli, imtihon dvigateli, fon ishlari, xavfsizlik, CI |

## Asosiy qarorlar (qisqacha)

1. **Farq qilish yo'li:** oddiy mock emas, balki aniqligi isbotlangan Writing/Speaking baholash + placement + o'qitish + original kontent.
2. **Baholovchi AI:** Claude Opus 5.5, kalibratsiya to'plami bilan; natija "taxminiy band ± oraliq" deb ko'rsatiladi.
3. **Speaking:** verbatim STT + akustik talaffuz bahosi + Claude.
4. **Asosiy taklif:** "7.0 Sprint" — davlat 7.0 uchun imtihon xarajatini qaytaradi.
5. **Ma'lumotlar:** PostgreSQL + Drizzle; ochiq ishga tushirishdan oldin O'zbekistonda hosting masalasi yurist bilan hal qilinadi.
