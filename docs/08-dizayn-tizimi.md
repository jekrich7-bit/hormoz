# 08 — Dizayn tizimi va asosiy ekranlar

Taklif qilinayotgan dizayn yo'nalishi. Siz tasdiqlaganingizdan keyin asosiy ekranlarning bosiladigan prototipini tayyorlayman, so'ng kodga o'tamiz.

---

## 1. Dizayn tamoyillari

1. **Ishonch:** toza, akademik ko'rinish; har bir ball yonida uning qanday hisoblangani ko'rsatiladi.
2. **Imtihondagi xotirjamlik:** imtihon ekranida ortiqcha hech narsa yo'q, haqiqiy kompyuter imtihoniga imkon qadar yaqin.
3. **Keyingi qadam doim aniq:** har bir sahifada bitta asosiy harakat bo'ladi.
4. **Telefon birinchi o'rinda** (imtihon rejimidan tashqari hamma joyda).
5. **Hamma uchun qulay:** WCAG 2.2 AA, klaviatura bilan boshqarish, kontrast sozlamalari.

## 2. Brend g'oyasi (ishchi)

- **Nom:** "Hormoz" (ishchi nom). Hormuz bo'g'ozi — dengiz yo'li, ya'ni "maqsadga olib boruvchi yo'l" g'oyasi.
- **Vizual metafora:** chuqur dengiz ko'ki (ishonch va bilim) + oltin rang (natija, 7.0 "oltin chegara").
- **Ohang:** "siz" deb murojaat qilinadi, do'stona va professional, qisqa gaplar. Masalan: "Ajoyib! Writing'da Coherence 0.5 ga oshdi." yoki "Bugun 3 ta vazifa qoldi, taxminan 35 daqiqa."
- **O'zbek harflari:** interfeys matnlarida to'g'ri belgilar ishlatiladi: `oʻ`, `gʻ` (U+02BB) va tutuq belgisi `ʼ` (U+02BC). Tanlangan shrift ularni qo'llab-quvvatlashi tekshiriladi.

## 3. Rang tokenlari

| Token | Yorug' rejim | Qorong'i rejim | Ishlatilishi |
|---|---|---|---|
| `--primary` | `#0F4C81` | `#5B9BD5` | Asosiy tugmalar, havolalar |
| `--primary-foreground` | `#FFFFFF` | `#0B1220` | Tugma matni |
| `--accent` | `#D99A1E` | `#F2C14E` | 7.0 maqsadi, yutuqlar (oz miqdorda) |
| `--success` | `#15803D` | `#4ADE80` | To'g'ri javob, o'sish |
| `--warning` | `#B45309` | `#FBBF24` | Taymer ogohlantirishi |
| `--danger` | `#B91C1C` | `#F87171` | Xato, noto'g'ri javob |
| `--background` | `#F8FAFC` | `#0B1220` | Sahifa foni |
| `--surface` | `#FFFFFF` | `#111A2E` | Kartochkalar |
| `--border` | `#E2E8F0` | `#22304A` | Chegaralar |
| `--text` | `#0F172A` | `#E5EAF2` | Asosiy matn |
| `--text-muted` | `#475569` | `#94A3B8` | Ikkinchi darajali matn |

**Band shkalasi ranglari** (badge va grafiklar uchun; matn har doim to'q rangda, kontrast ≥ 4.5:1):
`≤4.5` kulrang · `5–5.5` to'q sariq · `6–6.5` sariq · `7–7.5` yashil · `8–8.5` moviy · `9` binafsha.

Rang hech qachon yagona belgi bo'lmaydi: to'g'ri yoki noto'g'ri javob yonida doim ✓ / ✗ belgisi ham bo'ladi.

## 4. Tipografiya

| Element | Shrift | O'lcham / qator oralig'i |
|---|---|---|
| Interfeys | **Inter** (lotin + kirill) | 14–16px, 1.5 |
| Sarlavhalar | Inter Semibold | 20 / 24 / 30 / 36px |
| Imtihon matni | Tizimning oddiy sans-serif shrifti (Arial ga yaqin, haqiqiy imtihondagidek) | Standart 16px, o'quvchi 14–22px oralig'ida o'zgartira oladi, 1.6 |
| Raqamlar (ball, taymer) | Inter, `tabular-nums` | 16–48px |

## 5. O'lchamlar va shakllar

- **Bo'shliqlar:** 4px tizimi (4, 8, 12, 16, 24, 32, 48).
- **Burchak radiusi:** 8px (elementlar), 12px (kartochkalar).
- **Soya:** minimal, faqat ochiladigan oynalar va menyular uchun.
- **Animatsiya:** 150–200ms, `prefers-reduced-motion` hurmat qilinadi.
- **Ekran o'lchamlari (breakpoint):** 640 / 768 / 1024 / 1280px.

## 6. Komponentlar

**Tayyor (shadcn/ui):** Button, Input, Textarea, Select, Checkbox, Radio, Tabs, Dialog, Sheet, Dropdown, Tooltip, Toast (sonner), Badge, Progress, Avatar, Calendar, Command (admin uchun buyruq paneli), Form (react-hook-form + zod), DataTable (TanStack Table), Charts (Recharts).

**Platformaning o'z komponentlari:**

| Komponent | Vazifasi |
|---|---|
| `BandBadge` | Band rangli belgida, "taxminiy" belgisi bilan |
| `SkillRadar` | 4 ko'nikma bo'yicha radar diagramma |
| `CriterionBar` | Writing/Speaking mezoni: band + qisqa izoh |
| `ExamTimer` | Server bilan sinxron taymer, 10 va 5 daqiqa qolganda ogohlantirish |
| `QuestionNavigator` | Pastki savollar paneli: javob berilgan, bo'sh, belgilangan |
| `HighlightableText` | Matnni belgilash va eslatma qo'shish |
| `SplitPane` | Reading uchun o'lchami o'zgaradigan ikki panel |
| `OnceAudioPlayer` | Listening audio: bir marta ijro, ovoz balandligi, oldindan yuklash |
| `Recorder` | Mikrofondan yozish, ovoz darajasi indikatori, vaqt |
| `WordCounter` | Writing so'z hisoblagichi |
| `AnnotatedEssay` | Xatolar belgilangan insho: bosilganda izoh chiqadi |
| `TranscriptPlayer` | Audio bilan sinxron transkript |
| `FsrsCard` | Takrorlash kartochkasi: "Esladim / Qiyin / Unutdim" |
| `TodayPlan` | Bugungi reja ro'yxati |

## 7. Sahifa shablonlari (layout)

| Shablon | Tuzilishi |
|---|---|
| **Ommaviy** | Header (logo, Narxlar, Markazlar uchun, Kirish, "Bepul placement") + kontent + footer (disclaimer bilan) |
| **Talaba ilovasi** | Telefonda: pastda 5 ta tab (Bosh · Mock · Mashq · Lug'at · Profil). Kompyuterda: chap tomonda sidebar |
| **Imtihon** | To'liq ekran: yuqorida panel (bo'lim nomi, taymer, Sozlamalar, Yashirish), o'rtada kontent, pastda savollar paneli. Boshqa navigatsiya yo'q |
| **Admin / Markaz** | Chap tomonda guruhlangan sidebar, yuqorida qidiruv va buyruq paneli (⌘K), breadcrumb, kontent (jadval + filtrlar + o'ng tomonda ochiladigan panel) |

## 8. Asosiy ekranlarning sxemalari

### 8.1. Imtihon — Reading

```
┌──────────────────────────────────────────────────────────────────┐
│ Reading · Passage 2        ⏱ 38:12        [Aa Sozlamalar] [Yashir] │
├────────────────────────────────┬─────────────────────────────────┤
│ The Silk Road Economy          │ Questions 14–19                  │
│                                │ Choose the correct heading...    │
│ A  Lorem ipsum dolor sit amet  │                                  │
│    ▓▓▓▓▓ (belgilangan) ...     │ 14 Paragraph A   [ iv      ▾ ]   │
│ B  Consectetur adipiscing ...  │ 15 Paragraph B   [ Tanlang ▾ ] ⚑ │
│    ...                         │ ...                              │
│                      ◀ ║ ▶ (surish)                                │
├────────────────────────────────┴─────────────────────────────────┤
│ Part 1 ■■■■■■■■■■■■■  Part 2 ■■□□⚑□□□□□□□□  Part 3 □□□□□…   [✓ Tekshirish] │
└──────────────────────────────────────────────────────────────────┘
```

### 8.2. Imtihon — Writing

```
┌──────────────────────────────────────────────────────────────────┐
│ Writing · Task 2            ⏱ 41:05        [Aa Sozlamalar] [Yashir] │
├────────────────────────────────┬─────────────────────────────────┤
│ Some people believe that ...   │ ┌─────────────────────────────┐ │
│ Discuss both views and give    │ │ (imlo tekshiruvi o'chirilgan) │ │
│ your own opinion.              │ │                              │ │
│ Write at least 250 words.      │ └─────────────────────────────┘ │
│                                │                 So'zlar: 187     │
├────────────────────────────────┴─────────────────────────────────┤
│ [ Task 1 ]  [ Task 2 ● ]                                          │
└──────────────────────────────────────────────────────────────────┘
```

### 8.3. Imtihon — Speaking

```
┌──────────────────────────────────────────────────────────────┐
│ Speaking · Part 2                                  ⏱ 00:47     │
│                                                                │
│        🔊 "Describe a place you visited that ..."              │
│                                                                │
│  Tayyorgarlik: 1 daqiqa         ┌ Eslatmalar ───────────┐      │
│  ████████████░░░░  0:38         │ - Samarkand, 2023     │      │
│                                 │ - why special...      │      │
│                                 └───────────────────────┘      │
│        ● Yozilmoqda   ▁▃▅▇▅▃▁  01:12 / 02:00                   │
└──────────────────────────────────────────────────────────────┘
```

### 8.4. Natija — Writing feedback

```
┌──────────────────────────────────────────────────────────────┐
│ Writing Task 2 · Taxminiy band 6.0 (±0.5)    [Qayta yozish]    │
├──────────────────────────────────────────────────────────────┤
│ Task Response    6.0  ██████░░░  Pozitsiya bor, 2-fikr kam      │
│ Coherence        6.5  ██████▌░░  Abzaslar mantiqiy              │
│ Lexical          5.5  █████▌░░░  "important" 6 marta takror     │
│ Grammar          6.0  ██████░░░  Artikl xatolari (4)            │
├──────────────────────────────────────────────────────────────┤
│ Insho: In ⟦the modern world⟧ people ~~is~~ are ...              │
│        ↳ [Grammatika] Ega va kesim moslashuvi. "people are"     │
├──────────────────────────────────────────────────────────────┤
│ 6.5 ga chiqish uchun: 1) ...  2) ...  3) ...                    │
│ [Xatolarni jurnalga qo'shish]   [Mutaxassis tekshiruvi · 49k]   │
└──────────────────────────────────────────────────────────────┘
```

### 8.5. Admin — bosh panel

```
┌────────────┬─────────────────────────────────────────────────────┐
│ HORMOZ     │ Bosh panel                         [⌘K Qidiruv]  👤 │
│ ▸ Panel    ├─────────────────────────────────────────────────────┤
│ KONTENT    │ Bugun: 128 mock · 41 yangi to'lov · AI $6.20         │
│  Testlar   │ ┌──────────┐┌──────────┐┌──────────┐┌──────────┐    │
│  Savollar  │ │Daromad   ││Faol      ││Navbatda  ││AI aniqlik│    │
│  Media     │ │4.2 mln   ││1 240     ││ 12 ish   ││MAE 0.42  │    │
│  Darslar   │ └──────────┘└──────────┘└──────────┘└──────────┘    │
│ BAHOLASH   │ [Daromad grafigi — 30 kun]   [Konversiya voronkasi] │
│  Navbat    │ So'nggi muammolar: 3 ta savol bo'yicha shikoyat      │
│  Tekshiruv │                                                     │
│  Kalibrov. │                                                     │
│ ...        │                                                     │
└────────────┴─────────────────────────────────────────────────────┘
```

### 8.6. Admin — test konstruktori

```
┌───────────────────────────────────────────────────────────────────┐
│ Testlar › Academic Mock #07 › Reading › Passage 2    [Qoralama ▾] │
├──────────────────┬──────────────────────────────┬─────────────────┤
│ TUZILMA          │ SAVOLLAR GURUHI: 14–19       │ KO'RIB CHIQISH  │
│ ▾ Listening      │ Turi: [Matching Headings ▾]  │ (talaba ekrani  │
│ ▾ Reading        │ Ko'rsatma: [.............]   │  qanday ko'rinsa│
│   Passage 1 ✓    │ Sarlavhalar ro'yxati: i–viii │  aynan shunday) │
│   Passage 2 ●    │ 14 → A  javob: iv            │                 │
│   Passage 3      │ 15 → B  javob: vii           │                 │
│ ▸ Writing        │ [+ Savol] [AI qoralama ✨]   │                 │
│ ▸ Speaking       │ Tekshiruv: ✓ 1 javob ✓ dalil │                 │
└──────────────────┴──────────────────────────────┴─────────────────┘
```

### 8.7. Admin — mutaxassis tekshiruvi ish joyi

```
┌───────────────────────────────────────────────────────────────────┐
│ Tekshiruv navbati › #W-2381 (Task 2)       AI: 6.0   ⏱ o'rtacha 6 daq │
├─────────────────────────────────┬─────────────────────────────────┤
│ Topshiriq + insho matni          │ Sizning bahoyingiz (AI yashirin)│
│ (belgilash va izoh qo'shish)     │ TR  [5 5.5 6 6.5 7 ...]         │
│                                  │ CC  [ ... ]                     │
│                                  │ LR  [ ... ]   GRA [ ... ]       │
│                                  │ Izoh talabaga: [..........]     │
│                                  │ [Saqlash va keyingisi →]        │
└─────────────────────────────────┴─────────────────────────────────┘
```

Muhim: kalibratsiya uchun mutaxassis **avval o'zi baholaydi**, AI bahosini esa keyin ko'radi. Aks holda u AI bahosiga moslashib qoladi (anchoring).

## 9. Qulaylik va til

- Imtihonni to'liq klaviatura bilan boshqarish mumkin: Tab, strelkalar, raqamlar. Fokus doim ko'rinib turadi.
- Taymer ogohlantirishlari ekran o'quvchi dasturlar uchun ham e'lon qilinadi (`aria-live`).
- Interfeys tillari: `uz-Latn` (asosiy) va `ru`. Barcha matnlar i18n kalitlari orqali beriladi, kodga to'g'ridan-to'g'ri yozilmaydi.
- Sana, son va pul formatlari: `12 600 so'm`, `28.10.2026`.

## 10. Dizayn ishining keyingi qadamlari

1. Siz rang, shrift va ohangni tasdiqlaysiz yoki o'zgartirasiz.
2. Men 7 ta asosiy ekranning bosiladigan prototipini tayyorlayman: bosh sahifa, placement natijasi, bosh panel, Reading, Writing feedback, admin test konstruktori, tekshiruv ish joyi.
3. Prototipni 5–10 nafar haqiqiy o'quvchiga ko'rsatamiz va eng chalkash joylarni tuzatamiz.
4. Shundan keyingina shu tokenlar va komponentlar bilan kod yoziladi.
