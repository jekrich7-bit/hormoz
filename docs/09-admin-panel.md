# 09 — Admin panel: professional spetsifikatsiya

Admin panel uch vazifani bajaradi:
1. **Kontent fabrikasi:** test va darslarni yaratish, tekshirish va nashr qilish.
2. **Sifat markazi:** AI bahosini nazorat qilish, inson tekshiruvi va kalibratsiya.
3. **Biznes boshqaruvi:** foydalanuvchilar, to'lovlar, markazlar, marketing va analitika.

---

## 1. Asosiy tamoyillar

| Tamoyil | Amalda |
|---|---|
| Ruxsatlar serverda tekshiriladi | Har bir server action va API ruxsatni tekshiradi. UI'da tugmani yashirish yetarli emas |
| Hamma narsa audit qilinadi | Kim, nima, qachon, "oldin → keyin" qiymatlari va IP. Audit jurnalini o'chirib bo'lmaydi |
| Ikki bosqichli kirish (2FA) majburiy | Barcha admin rollari uchun (TOTP) |
| Yumshoq o'chirish va versiyalar | Test va savollar o'chirilmaydi, arxivlanadi; har o'zgarish versiya sifatida saqlanadi va solishtiriladi |
| Shaxsiy ma'lumotlar himoyasi | Support rolida telefon raqami qisman yashiriladi; eksport faqat ruxsat bilan va audit qilinadi |
| Tezkor ishlash | ⌘K buyruq paneli, klaviatura yorliqlari, ommaviy amallar, saqlangan filtrlar |
| "Boshqa foydalanuvchi sifatida ko'rish" | Faqat o'qish rejimida, ekranda ogohlantirish bilan va audit qilinadi |

---

## 2. Ruxsatlar matritsasi

Belgilar: **C** yaratish · **R** ko'rish · **U** tahrirlash · **D** arxivlash · **A** tasdiqlash/nashr · — ruxsat yo'q

| Modul | Super admin | Kontent menejeri | Kontent muharriri | Mutaxassis | Support | Moliya |
|---|---|---|---|---|---|---|
| Bosh panel | R | R | R (kontent) | R (o'zi) | R | R (moliya) |
| Foydalanuvchilar | CRUD | R | — | — | RU (cheklangan) | R |
| Testlar va savollar | CRUDA | CRUDA | CRU | R | R | — |
| Media, TTS | CRUD | CRUD | CRU | — | — | — |
| Darslar, lug'at | CRUDA | CRUDA | CRU | — | — | — |
| AI kontent studiyasi | ✓ | ✓ | ✓ | — | — | — |
| Baholash navbati | CRUD | R | — | RU (o'ziga biriktirilgan) | R | — |
| Kalibratsiya, eval | CRUD | R | — | RU (baholash) | — | — |
| Prompt reestri | CRUDA | R | — | — | — | — |
| Guruhlar, kogortalar | CRUD | CRUD | — | — | RU | — |
| B2B markazlar | CRUD | R | — | — | RU | RU |
| To'lovlar, tariflar | CRUD | — | — | — | R | CRUD (qaytarish: A) |
| Marketing | CRUD | CRU | — | — | — | — |
| Murojaatlar | CRUD | RU (kontent shikoyatlari) | RU (kontent shikoyatlari) | RU (apellyatsiyalar) | CRUD | R (to'lov) |
| Analitika | R | R | — | — | R | R |
| Sozlamalar, rollar | CRUD | — | — | — | — | — |
| Audit jurnali | R | — | — | — | — | — |

Markaz admini va o'qituvchi admin panelga kirmaydi: ular faqat **markaz portalida** o'z ma'lumotlarini ko'radi.

---

## 3. Modullar

### 3.1. Bosh panel

- **KPI kartochkalari:** bugungi daromad, faol obunachilar (MRR), topshirilgan mocklar, navbatdagi ishlar, AI xarajati (kunlik), AI aniqligi (MAE, so'nggi 30 kun).
- **Grafiklar:** daromad (30 kun), konversiya voronkasi (ro'yxatdan o'tish → placement → pullik), ko'nikmalar bo'yicha o'rtacha ballar.
- **Ogohlantirishlar:** muvaffaqiyatsiz ishlar navbati, savollar bo'yicha shikoyatlar, AI xarajati limitdan oshishi, to'lov provayderidagi xatolar.

### 3.2. Foydalanuvchilar

- **Jadval:** qidiruv (ism, Telegram, telefon), filtrlar (tarif, daraja, markaz, ro'yxatdan o'tgan sana, faollik), ommaviy amallar.
- **Profil kartochkasi (tablar):**
  - Umumiy: maqsad, imtihon sanasi, daraja va hokazo
  - Urinishlar va natijalar
  - Obuna, limitlar va to'lovlar
  - Murojaatlar
  - Faollik tarixi
  - Audit
- **Amallar:** limitni qo'lda oshirish (sabab bilan), bonus berish, bloklash, "foydalanuvchi sifatida ko'rish", ma'lumotlarni eksport qilish yoki o'chirish (maxfiylik so'rovi bo'yicha).

### 3.3. Kontent → Testlar (test konstruktori)

Tuzilma: **Test → Bo'lim (L/R/W/S) → Qism (Part / Passage) → Savollar guruhi → Savol**

- **Chap panel:** test tuzilmasi daraxti, har bo'limning holati.
- **Markaz:** tanlangan element muharriri.
  - *Listening qismi:* skript, audio (yuklash yoki TTS orqali yaratish), savollar guruhlari.
  - *Reading passage:* matn (A, B, C... abzaslar), lug'atlik va so'z soni hisoblagichi, guruhlar.
  - *Savollar guruhi:* turi (quyidagi ro'yxatdan), ko'rsatma, umumiy material (jadval, xarita, sarlavhalar ro'yxati), savollar oralig'i.
  - *Savol:* matn, variantlar, to'g'ri javob(lar), qabul qilinadigan variantlar, so'z chegarasi, **matndagi dalil** (javob joyini belgilash), izoh (o'zbekcha va inglizcha), CEFR, ko'nikma teglari.
- **O'ng panel:** talaba ekranida qanday ko'rinishi (jonli ko'rinish).
- **Avtomatik tekshiruvlar:** savollar raqamlari ketma-ket, har savolda javob kaliti bor, javob matnda belgilangan, so'z chegarasi javobga mos, Listening audio uzunligi va hokazo.
- **Test spetsifikatsiyasi (blueprint)** bilan solishtirish: masalan, "Reading'da 40 savol, kamida 4 xil savol turi". Spetsifikatsiyaga mos kelmasa, testni nashr qilib bo'lmaydi.

**Qo'llab-quvvatlanadigan savol turlari:**

| Listening | Reading | Writing | Speaking |
|---|---|---|---|
| Form / note / table / flow-chart completion | Multiple choice (bitta yoki bir nechta javob) | Task 1 Academic (grafik, jadval, jarayon, xarita, rasm bilan) | Part 1 savollar to'plami |
| Sentence completion | TRUE / FALSE / NOT GIVEN | Task 2 (opinion, discussion, problem-solution, two-part) | Part 2 cue card (rasmiy shakl) |
| Multiple choice | YES / NO / NOT GIVEN | | Part 3 savollar to'plami |
| Matching | Matching headings | | |
| Map / plan / diagram labelling | Matching information / features / sentence endings | | |
| Short answer | Sentence / summary / note / table / flow-chart completion | | |
| | Diagram label completion, short answer | | |

### 3.4. Kontent → Savollar banki

- Barcha savollar bir joyda: filtrlar (ko'nikma, tur, CEFR, holat, test, statistika).
- **Savol statistikasi** (pilot va real foydalanishdan keyin):
  - to'g'ri javob berganlar foizi (qiyinlik)
  - kuchli va zaif o'quvchilarni ajrata olishi (diskriminatsiya, nuqta-biserial korrelyatsiya)
  - chalg'ituvchilar tahlili (har variantni tanlaganlar ulushi)
  - o'rtacha sarflangan vaqt
- **Avtomatik bayroqlar:** juda oson (>95%), juda qiyin (<10%), diskriminatsiya manfiy (ehtimol kalit xato), tez-tez shikoyat qilinadi.
- Placement savollari uchun: IRT parametrlari (`b`, `a`), ko'rsatilishlar soni, oxirgi kalibratsiya sanasi.

### 3.5. Kontent → AI kontent studiyasi

Claude yordamida **qoralama** yaratiladi, lekin yakuniy qarorni har doim inson qabul qiladi.

1. Muharrir spetsifikatsiya beradi: ko'nikma, qism, mavzu, CEFR, so'z soni, savol turlari va soni.
2. Claude matn va savollar qoralamasini javob kalitlari va dalillar bilan qaytaradi (structured output).
3. Avtomatik tekshiruvlar ishga tushadi: mavjud kontent bilan o'xshashlik, javob matnda bormi, so'z chegarasi.
4. Muharrir tahrirlaydi → tekshiruvga yuboradi.
5. Har bir qoralamaning `prompt_version` va kim tomonidan qabul qilingani saqlanadi.

### 3.6. Kontent → Media kutubxona

- Audio va rasmlar: yuklash, teglar, qaysi testda ishlatilgani.
- **TTS generator:** skript → spikerlarni tanlash (aksent, jins, yosh) → har bir replikaga ovoz berish → pauzalar → bitta audio fayl. Natijani muharrir tinglab tasdiqlaydi.
- Grafik va jadval yaratish (Writing Task 1 uchun): ma'lumotlar jadvali kiritiladi va grafik shu ma'lumotdan avtomatik chiziladi.

### 3.7. Kontent → Darslar, kurslar va lug'at

- **Yo'llar (L0–L6) → modullar → darslar → mashq va darvoza testi.**
- Dars muharriri: matn, video havolasi, misollar, tekshiruv savollari (savollar bankidan).
- **Lug'at to'plamlari:** so'z, ma'no (o'zbekcha va inglizcha), misol, audio, CEFR, darajasi; CSV orqali import.

### 3.8. Kontent sifati (workflow)

**Holatlar:** `Qoralama → Tekshiruvda → Tuzatish kerak → Tasdiqlangan → Pilot → Faol → Arxiv`

| O'tish | Kim bajaradi | Shart |
|---|---|---|
| Qoralama → Tekshiruvda | Muharrir | Avtomatik tekshiruvlardan o'tgan |
| Tekshiruvda → Tasdiqlangan / Tuzatish kerak | Kontent menejeri (muallifdan boshqa odam) | Tekshiruv ro'yxati to'ldirilgan |
| Tasdiqlangan → Pilot | Kontent menejeri | Pilot guruhi tanlangan (30–50 kishi) |
| Pilot → Faol | Kontent menejeri | Statistikada qizil bayroq yo'q |
| Faol → Arxiv | Kontent menejeri | Sabab ko'rsatiladi |

- Har bir element bo'yicha izohlar, tekshiruvchini biriktirish, versiyalarni solishtirish (diff).
- **Talabalarning "savolda xato bor" shikoyatlari** shu yerga keladi.

### 3.9. Baholash → AI navbati

- Ishlar: Writing, Speaking, TTS, Batch. Holatlari: kutmoqda, bajarilmoqda, tayyor, xato.
- Xatolarni qayta ishga tushirish, o'rtacha bajarilish vaqti, narx (token va daqiqa).

### 3.10. Baholash → Inson tekshiruvi

- **Navbatga tushadigan ishlar:**
  1. Pullik "mutaxassis tekshiruvi"
  2. AI ishonchi past yoki takroriy baholar orasida katta farq
  3. Tasodifiy 5% nazorat namunasi
  4. Apellyatsiyalar
- **Ish joyi** (08-hujjat, 8.7): mutaxassis AI bahosini ko'rmasdan baholaydi. Kalibratsiya namunalari ko'r (blind) holda beriladi.
- **Navbat qoidalari:** ishlarni biriktirish, xizmat muddati (masalan, 24 soat), mutaxassis bo'yicha statistika (hajm, o'rtacha vaqt, boshqa mutaxassislar bilan moslik), mutaxassislarga to'lov hisobi.

### 3.11. Baholash → Kalibratsiya va eval

- **Kalibratsiya to'plamlari:** namunalarni yuklash, 2 nafar mutaxassis tomonidan ko'r holda baholash, mutaxassislar orasidagi moslik, kelishmovchilikda uchinchi mutaxassis.
- **Eval natijalari:** har bir `model_id + prompt_version` uchun QWK, MAE, ±0.5 moslik, mezonlar bo'yicha xatolik, vaqt bo'yicha trend.
- **Haqiqiy natijalar:** foydalanuvchilar yuklagan TRF'lar (tekshirilgan) va bizning bashorat → ochiq aniqlik hisoboti uchun ma'lumot.

### 3.12. Baholash → Prompt reestri

- Har bir prompt (Writing T1, Writing T2, Speaking, kontent studiyasi va hokazo) versiyalari bilan saqlanadi: matn, model, sozlamalar, rubrika versiyasi.
- Versiyalarni solishtirish (diff), eval natijasi, **faollashtirish** (avval trafikning 10 foizida sinab ko'rish, keyin 100%), orqaga qaytarish.
- Qoida: eval'dan o'tmagan versiyani faollashtirib bo'lmaydi.

### 3.13. Placement

- **Test tuzilmasi:** bo'limlar, savollar soni, to'xtash qoidasi (SE, maksimal savollar soni).
- **Savollar parametrlari** va kalibratsiya: oflayn `mirt` natijalarini import qilish, ko'rsatilishlarni nazorat qilish.
- **Natija chegaralari:** θ → CEFR → band o'tkazish jadvali, versiyalari bilan.

### 3.14. Guruhlar va kogortalar

- Guruh yaratish: daraja, jadval, o'qituvchi, hajm, boshlanish sanasi.
- **Avtomatik guruhlash tavsiyasi** (02-hujjat, 4.2) → admin tasdiqlaydi.
- Har 4 haftada **qayta guruhlash takliflari** ro'yxati, davomat va jonli dars havolalari.

### 3.15. B2B markazlar

- Markazlar ro'yxati, shartnoma, litsenziya turi, o'rinlar va ishlatilgan tekshiruvlar.
- Markaz adminlari, logotip (portal uchun), hisob-fakturalar va to'lovlar.

### 3.16. Savdo va to'lovlar

- **Tariflar:** narx, muddat, limitlar (mock, Writing, Speaking), imkoniyatlar ro'yxati. Versiyalanadi: eski obunachilar eski shartda qoladi.
- **Promo kodlar:** foiz yoki summa, muddat, foydalanish limiti, kampaniya.
- **Tranzaksiyalar:** provayder (Payme / Click / Uzum), holat, summa, provayderning asl so'rov ma'lumotlari, foydalanuvchi.
- **Qaytarishlar:** so'rov → moliya tasdiqlaydi → provayder orqali qaytariladi; sabab majburiy.
- **Solishtirish:** kunlik hisobot — bizdagi tranzaksiyalar bilan provayder hisoboti mos kelishi tekshiriladi.
- **Referal to'lovlar:** hamkor o'qituvchilarga beriladigan ulushlar.

### 3.17. Marketing

- **Telegram xabarnomalar:** segment (daraja, tarif, faollik, imtihon sanasi), vaqtini belgilash, A/B variantlar, yetkazilish va bosilish statistikasi.
- **Mock Day:** tadbir yaratish (test, sana, vaqt), ro'yxatdan o'tganlar, reytingni nashr qilish.
- **Natijalar:** tasdiqlangan TRF va yozma rozilik bilan.
- **Kampaniyalar:** UTM havolalar va har bir kanal bo'yicha konversiya.

### 3.18. Murojaatlar

- Turlari: texnik muammo, to'lov, savolda xato, ballga apellyatsiya.
- Holatlar, mas'ul xodimni biriktirish, xizmat muddati (SLA), tayyor javob shablonlari, foydalanuvchi profiliga bog'lash.

### 3.19. Analitika

- **Voronka:** tashrif → ro'yxatdan o'tish → placement → birinchi mock → to'lov.
- **Saqlanish kogortalari** (haftalik), MRR, churn, ARPU, LTV.
- **AI xarajati:** funksiya, tarif va foydalanuvchi bo'yicha; eng ko'p xarajat qiladigan foydalanuvchilar.
- **O'quv natijalari:** o'qilgan soatlar va band o'sishi o'rtasidagi bog'liqlik.
- CSV eksport (ruxsat bilan).

### 3.20. Sozlamalar va tizim

- **Admin foydalanuvchilar va rollar**, 2FA siyosati.
- **Funksiya bayroqlari** (feature flags): yangi funksiyani asta-sekin yoqish.
- **Interfeys matnlari** (uz/ru): kodsiz tahrirlash.
- **Huquqiy sahifalar**, bildirishnoma shablonlari (Telegram, email).
- **Integratsiyalar holati:** kalitlarning o'zi faqat serverdagi muhit o'zgaruvchilarida saqlanadi; adminda faqat "ulangan / xato" holati ko'rsatiladi.
- **Audit jurnali:** filtr va qidiruv.
- **Fon ishlari va tizim holati**, zaxira nusxalar.
- **Maxfiylik so'rovlari:** ma'lumotlarni eksport qilish yoki o'chirish.

---

## 4. Admin panel qaysi sprintda quriladi

| Sprint | Admin modullari |
|---|---|
| 0 | Admin qobig'i (layout, sidebar, ⌘K), rollar va ruxsatlar, 2FA, audit jurnali |
| 1 | Test konstruktori (Reading), savollar banki, media kutubxona |
| 2 | Listening muharriri + TTS generator, blueprint tekshiruvi, kontent workflow |
| 3 | Prompt reestri, AI navbati, kalibratsiya to'plamlari, eval paneli, AI kontent studiyasi |
| 4 | Inson tekshiruvi ish joyi va navbat qoidalari |
| 5 | Foydalanuvchilar moduli, bosh panel KPI'lari |
| 6 | Tariflar, promo kodlar, tranzaksiyalar, qaytarishlar, murojaatlar |
| 7–8 | Darslar, lug'at to'plamlari, guruhlar va kogortalar |
| 9–10 | B2B markazlar, marketing (Telegram, Mock Day), analitika |
| 11–12 | Placement parametrlari va kalibratsiya, savol statistikasi, solishtirish (reconciliation) |
