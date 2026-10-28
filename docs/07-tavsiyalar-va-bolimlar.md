# 07 — Tavsiyalarim va platforma bo'limlari

Bu hujjat "nima quramiz va nima uchun" degan savolga javob beradi. Dizayn 08-hujjatda, admin panel 09-hujjatda, kod skeleti 10-hujjatda, sprintlar esa 05-hujjatda.

---

## 1. Asosiy tavsiyalarim (qaror talab qiladi)

| # | Tavsiya | Nega |
|---|---|---|
| 1 | **MVP fokusi: Academic + to'liq kompyuter mock + Writing AI tekshiruvi** | O'zbekistonda faqat kompyuter formati bor ✅. Writing eng zaif ko'nikma (5.81) ✅ va raqobatchilar uni eng yomon qiladi |
| 2 | **Admin panel birinchi sprintdan quriladi** | Eng katta to'siq — kontent. Admin panel kontent ishlab chiqaradigan "fabrika" bo'ladi: test konstruktori, AI qoralama, tekshiruv va pilot |
| 3 | **Inson tekshiruvi ish joyi boshidanoq bo'ladi** | Har bir mutaxassis tekshiruvi bir vaqtning o'zida kalibratsiya ma'lumoti bo'lib qoladi. Bu AI aniqligini o'lchash va oshirish uchun xom ashyo |
| 4 | **Bitta Next.js ilova + alohida worker jarayoni** | Bir kishi uchun eng sodda yo'l. Microservice'lar kerak emas |
| 5 | **PostgreSQL + Drizzle + pg-boss (navbat)** | Redis'siz, bitta baza. O'zbekistondagi serverga ko'chirish oson |
| 6 | **Telegram — asosiy kirish usuli va bildirishnomalar kanali** | Auditoriya Telegram'da |
| 7 | **Talaba ilovasi mobil uchun, imtihon esa kompyuter uchun** | Mashq va lug'at telefonda qilinadi. To'liq mock haqiqiy imtihondagidek kompyuterda bo'lishi kerak |
| 8 | **Interfeys tili: o'zbek (lotin) asosiy, rus ikkinchi; test kontenti ingliz tilida** | Bozor |
| 9 | **Aniqlikni o'lchab, ochiq e'lon qilish** | Bu asosiy marketing quroli va ishonch manbai |
| 10 | **Brend nomida "IELTS" so'zi bo'lmaydi** | Tovar belgisi xavfi |

### Hozircha QURMAYMIZ (keyinga qoldiriladi)

- Alohida mobil ilova (PWA yetarli)
- Real vaqtda jonli ovozli AI imtihon oluvchi (avval yozib olingan savollar ishlatiladi)
- Adaptiv placement (CAT): savollar kalibrlanmaguncha qat'iy shakldagi test ishlatiladi
- Mutaxassislar marketplace'i, gamifikatsiya (ball, nishon), ijtimoiy tarmoq funksiyalari
- General Training (2-bosqichga qoldiriladi)

---

## 2. Foydalanuvchi rollari

| Rol | Kim | Qayerda ishlaydi |
|---|---|---|
| Mehmon | Ro'yxatdan o'tmagan tashrif buyuruvchi | Ommaviy sahifalar, bepul placement'ning boshlanishi |
| Talaba | Asosiy mijoz | Talaba ilovasi + imtihon rejimi |
| O'qituvchi | Markaz yoki kogorta o'qituvchisi | Markaz portali (o'z guruhlari) |
| Markaz admini | B2B mijoz | Markaz portali (butun markaz) |
| Mutaxassis-baholovchi | Sertifikatli ekspert | Admin → Tekshiruv ish joyi |
| Kontent muharriri | Test va dars yozuvchi | Admin → Kontent |
| Kontent menejeri | Kontentni tasdiqlaydi va nashr qiladi | Admin → Kontent + Sifat |
| Qo'llab-quvvatlash | Support | Admin → Foydalanuvchilar, Murojaatlar |
| Moliya | Buxgalter | Admin → To'lovlar |
| Super admin | Siz | Hammasi |

Ruxsatlar matritsasi 09-hujjatda.

---

## 3. Sayt xaritasi

```
OMMAVIY SAHIFALAR
/                      Bosh sahifa
/placement             Bepul darajani aniqlash (boshlash)
/narxlar               Tariflar
/markazlar             B2B: o'quv markazlari uchun
/aniqlik               Ochiq aniqlik hisoboti (AI va haqiqiy IELTS)
/qollanmalar           Blog va qo'llanmalar (SEO)
/savollar              FAQ
/huquqiy/*             Oferta, maxfiylik, disclaimer, qaytarish siyosati

TALABA ILOVASI (kirgandan keyin)
/app                   Bosh panel: "Bugungi reja"
/app/placement         Placement va natijalari
/app/mock              Mock testlar: to'liq yoki bo'lim bo'yicha
/app/mashq             Mashqlar: ko'nikma va savol turi bo'yicha
/app/kurs              Mening yo'lim (L0–L6): darslar
/app/lugat             Lug'at (FSRS takrorlash)
/app/xatolar           Xatolar jurnali
/app/natijalar         Natijalar tarixi va progress
/app/natijalar/[id]    Bitta urinishning batafsil hisoboti
/app/guruh             Guruhim: jadval, jonli darslar, e'lonlar
/app/mock-day          Haftalik Mock Day va reyting
/app/obuna             Tarif, to'lovlar, limitlar
/app/sozlamalar        Profil, maqsad, til, bildirishnomalar
/app/yordam            Yordam va murojaat

IMTIHON REJIMI (to'liq ekran, navigatsiyasiz)
/imtihon/[attemptId]   Listening → Reading → Writing → Speaking

MARKAZ PORTALI (B2B)
/markaz                Panel
/markaz/oquvchilar     O'quvchilar va taklif havolalari
/markaz/guruhlar       Guruhlar va o'qituvchilar
/markaz/topshiriqlar   Test va vazifa tayinlash
/markaz/hisobotlar     Natijalar, progress, eksport
/markaz/litsenziya     Tarif, o'rinlar, hisob-fakturalar

ADMIN PANEL
/admin/*               09-hujjatda batafsil
```

---

## 4. Bo'limlarning batafsil tavsifi

### 4.1. Bosh sahifa (`/`)

**Maqsad:** tashrif buyuruvchini bepul placement'ga olib kirish.

Bloklar ketma-ketligi:
1. **Hero:** "Haqiqiy imtihondagidek mock va aniq Writing/Speaking bahosi". Asosiy tugma: **"Darajamni bepul aniqlash"**. Ikkinchi tugma: "Namuna hisobotni ko'rish".
2. **Ishonch qatori:** "X ta mock topshirildi", "AI va mutaxassis bahosi orasidagi farq: ±0.4 band" (faqat haqiqiy o'lchangandan keyin ko'rsatiladi).
3. **Qanday ishlaydi:** 3 qadam — placement → shaxsiy yo'l → haftalik mock.
4. **Namuna hisobot:** Writing feedback'ining interaktiv ko'rinishi.
5. **7.0 bloki:** "7.0 olsangiz, davlat imtihon pulini qaytaradi", qo'llanmaga havola.
6. **Tariflar** (qisqa ko'rinish).
7. **Natijalar:** faqat tasdiqlangan TRF bilan, rozilik olingan holda.
8. **FAQ** + disclaimer.

### 4.2. Placement (`/placement` → `/app/placement`)

- Ro'yxatdan o'tmasdan boshlash mumkin. Natijani ko'rish uchun Telegram orqali kirish kerak: bu lid yig'ish nuqtasi.
- Bosqichlar: maqsad va imtihon sanasi → grammatika va lug'at (15–20 daq) → mini Reading va Listening → qisqa Writing (10 daq) → 3 ta Speaking savoli.
- **Natija sahifasi:**
  - CEFR darajasi va umumiy taxminiy band oralig'i
  - Ko'nikmalar bo'yicha radar diagramma
  - 3 ta asosiy muammo
  - Tavsiya etilgan yo'l va maqsadgacha taxminiy hafta soni
  - Tugmalar: "Yo'limni boshlash" (Pro yoki Sprint) va "PDF hisobotni yuklab olish"

### 4.3. Bosh panel (`/app`) — "Bugungi reja"

```
┌─────────────────────────────────────────────┐
│ Salom, Aziz!  Maqsad: 7.0 · Imtihongacha 46 kun │
├─────────────────────────────────────────────┤
│ BUGUNGI REJA (≈75 daq)              2/5 ✓   │
│ ☑ Lug'at takrorlash · 18 karta     (10 daq) │
│ ☑ Dars: Matching Headings          (15 daq) │
│ ☐ Mashq: 13 savol                  (20 daq) │
│ ☐ Writing Task 2 + qayta yozish    (30 daq) │
│ ☐ Xatolar: 6 karta                  (5 daq) │
├─────────────────────────────────────────────┤
│ JORIY DARAJA   L 6.5  R 7.0  W 5.5  S 6.0   │
│ Progress grafigi (oxirgi 8 hafta)           │
├─────────────────────────────────────────────┤
│ Keyingi to'liq mock: shanba · Mock Day 19:00 │
│ Limitlar: Writing 5/8 · Speaking 6/8         │
└─────────────────────────────────────────────┘
```

Reja tizim tomonidan avtomatik tuziladi: eng zaif ko'nikma, imtihon sanasi va FSRS navbatiga qarab.

### 4.4. Mock testlar (`/app/mock`)

- **Turlar:** to'liq mock (2 soat 45 daqiqa + Speaking), bitta bo'lim, "Mock Day" (hamma bir vaqtda).
- **Har bir test kartochkasida:** nomi, qiyinligi, topshirganlar soni, o'rtacha ball, sizning oxirgi natijangiz.
- **Boshlashdan oldin:** quloqchin va mikrofon tekshiruvi, qoidalar, "kompyuterdan topshirish tavsiya etiladi" ogohlantirishi.

### 4.5. Imtihon rejimi (`/imtihon/[attemptId]`)

Haqiqiy kompyuter imtihoni funksiyalarini takrorlaydi ✅:
- Tepada o'rtada taymer: oxirgi 10 va 5 daqiqada ogohlantirish.
- Sozlamalar: shrift o'lchami, fon va matn rangi (kontrast).
- Matnni belgilash va eslatma qo'shish (sichqonchaning o'ng tugmasi yoki uzoq bosish).
- Savolni "keyinroq qaytish" uchun belgilash (flag).
- Pastda savollar paneli: javob berilgan, berilmagan va belgilangan savollar ko'rinib turadi.
- "Yashirish" tugmasi (pauza emas: taymer to'xtamaydi).
- Listening: ovoz balandligi, audio faqat bir marta ijro etiladi, oxirida 2 daqiqa tekshirish uchun vaqt.
- Reading: ekran ikkiga bo'linadi (matn | savollar), chegarani surib o'lchamini o'zgartirish mumkin.
- Writing: so'z hisoblagichi, brauzerning imlo tekshiruvi va avtotuzatish **o'chirilgan**, Task 1 / Task 2 o'rtasida o'tish.
- Speaking: imtihon oluvchi ovozi, Part 2 uchun 1 daqiqa tayyorgarlik va eslatma maydoni, yozish indikatori.
- Har 5–10 soniyada avtomatik saqlash; internet uzilsa "Ulanish tiklanmoqda..." xabari chiqadi va javoblar lokal saqlanadi.

### 4.6. Natija hisoboti (`/app/natijalar/[id]`)

- Umumiy band va 4 ko'nikma. Writing va Speaking uchun **"taxminiy, ±0.5"** belgisi.
- **Listening/Reading:** har bir savol — sizning javobingiz, to'g'ri javob, matndagi dalil (belgilangan holda), savol turi bo'yicha statistika ("TRUE/FALSE/NOT GIVEN: 4/7").
- **Writing:** 4 mezon; matnda xatolar belgilangan (turi va tuzatish bilan); har mezon bo'yicha "keyingi bandga chiqish uchun nima kerak"; **"Qayta yozish"** tugmasi; namunaviy yuqoriroq bandli versiya.
- **Speaking:** audio pleyer + transkript (vaqt belgilari bilan), fluency ko'rsatkichlari, talaffuzi zaif so'zlar, 4 mezon.
- Har bir xato **"Xatolar jurnaliga qo'shish"** tugmasi orqali FSRS kartochkasiga aylanadi.
- "Mutaxassis tekshiruvini so'rash" (pullik qo'shimcha xizmat).
- "Natija noto'g'rimi?" — murojaat yuborish (admin'ga apellyatsiya).

### 4.7. Mashqlar (`/app/mashq`)

Ko'nikma → savol turi → qiyinlik bo'yicha filtrlanadi. Masalan: Reading → Matching Headings → B2. Har bir mashq qisqa (5–15 daqiqa), natija darhol chiqadi va zaif savol turlari ajratib ko'rsatiladi.

### 4.8. Kurs — "Mening yo'lim" (`/app/kurs`)

- Placement natijasiga ko'ra L0–L6 yo'lidan biri tanlanadi (02-hujjat).
- Modullar → mikro-darslar (video yoki matn + misollar + 5 ta tekshiruv savoli).
- Har modul oxirida "darvoza testi": o'tmaguncha keyingi modul ochilmaydi (mastery learning).

### 4.9. Lug'at va Xatolar jurnali (`/app/lugat`, `/app/xatolar`)

- **FSRS takrorlash:** kartochkaning old tomonida so'z yoki xato gap, orqa tomonida ma'no, misol, talaffuz (audio) va o'zbekcha izoh.
- **Manbalar:** yo'lga mos tayyor to'plamlar (L2 uchun 3 000 so'z, L3–L4 uchun AWL) + o'quvchining o'z xatolari.

### 4.10. Guruhim (`/app/guruh`)

Kogorta jadvali, jonli dars havolasi (Zoom yoki Google Meet), o'qituvchi e'lonlari, guruh reytingi (ixtiyoriy) va qayta guruhlash haqida bildirishnoma.

### 4.11. Obuna (`/app/obuna`)

Joriy tarif, limitlar (progress bar bilan), to'lov tarixi, Payme / Click / Uzum orqali to'lash, promo kod, tarifni o'zgartirish.

### 4.12. Markaz portali (`/markaz`)

- **O'quvchilar:** taklif havolasi yoki kodi orqali qo'shish; har o'quvchining darajasi, oxirgi natijasi, faolligi.
- **Guruhlar:** tizim tavsiya qilgan guruhlash (02-hujjat, 4-bo'lim) + qo'lda o'zgartirish.
- **Topshiriqlar:** mock yoki mashqni guruhga muddat bilan tayinlash.
- **Hisobotlar:** guruh bo'yicha o'rtacha ballar, o'sish, eng zaif savol turlari; Excel'ga eksport.
- **Litsenziya:** o'rinlar soni, ishlatilgan tekshiruvlar, hisob-fakturalar.
