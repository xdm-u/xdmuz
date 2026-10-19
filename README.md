# XDM PRINT — Marketplace / buyurtma sayti

Next.js + TypeScript + Supabase asosidagi ixcham XDM PRINT starter loyihasi.

## Funksiyalar
- Email/parol orqali ro‘yxatdan o‘tish va kirish
- Mahsulotlar katalogi
- Buyurtma modal oynasi: ism, telefon, miqdor, email, izoh
- Buyurtmani PostgreSQL bazasiga saqlash
- Ixtiyoriy Resend email xabarnomasi
- Admin panel: mahsulot qo‘shish, tahrirlash/narxini o‘zgartirish, yashirish va o‘chirish
- Supabase RLS orqali admin va mijoz ruxsatlarini ajratish
- Responsive, ixcham dizayn
- SVG logotip va favicon
- Yorug‘ / qorong‘i rejim, tanlov brauzerda saqlanadi
- Admin panelida buyurtmalar ro‘yxati va holatni yangilash

> Bu XDM PRINT buyurtma MVP versiyasi. Bu versiyada sotuvchilar marketplace kabineti, onlayn to‘lov va yetkazib berish integratsiyasi hali yo‘q. XDM PRINT bosma xizmatiga mos ravishda mijoz buyurtma so‘rovi yuboradi va operator bog‘lanadi.

## 1. Talablar
- Node.js 20+
- Supabase bepul loyiha
- (Ixtiyoriy) Resend hisob va tasdiqlangan yuboruvchi domen/email

## 2. O‘rnatish
```bash
npm install
cp .env.example .env.local
```
`.env.local` faylini Supabase Project URL, anon/publishable key va server uchun `SUPABASE_SERVICE_ROLE_KEY` bilan to‘ldiring. Service-role key faqat serverda ishlatiladi; hech qachon `NEXT_PUBLIC_` bilan boshlamasin va Git’ga yuklanmasin.

## 3. Bazani yaratish
1. Supabase → SQL Editor’ni oching.
2. `supabase/schema.sql` faylining mazmunini ishga tushiring.
3. Supabase → Authentication → Providers ichida Email provider yoqilganini tekshiring.
4. Developmentda email confirmation’ni yoqib qo‘ysangiz, foydalanuvchi emailini tasdiqlashi kerak. Sinov uchun Supabase Auth sozlamalarini moslang.

## 4. Admin hisobini berish
1. Saytda `/register` orqali o‘zingizning hisobingizni yarating.
2. Supabase → Authentication → Users’dan o‘zingizning User UID’ingizni oling.
3. SQL Editor’da faqat o‘zingizning UID bilan ishga tushiring:
```sql
update public.profiles set role = 'admin'
where id = 'YOUR_AUTH_USER_UUID';
```
Hech qachon oddiy foydalanuvchiga admin rolini ochiq ro‘yxatdan o‘tish orqali bermang.

## 5. Buyurtma emailini sozlash (ixtiyoriy, lekin tavsiya etiladi)
`.env.local`:
```dotenv
RESEND_API_KEY=re_...
ORDER_NOTIFICATION_EMAIL=buyurtmalar@your-domain.uz
FROM_EMAIL=XDM PRINT <verified-sender@your-domain.uz>
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```
Resend’da `FROM_EMAIL` uchun tasdiqlangan yuboruvchi talab qilinishi mumkin. API key faqat serverda qoladi. Bu o‘zgaruvchilarga `NEXT_PUBLIC_` qo‘shmang. Email yuborish xatosi buyurtmani saqlashni bekor qilmaydi; server logini tekshiring.

## 6. Dizayn
- `public/xdm-print-logo.svg` — sayt logotipi
- `public/favicon.svg` — brauzer belgisi
- Navigatsiyadagi oy/quyosh tugmasi oq-qora (light/dark) rejimni almashtiradi
- Admin panelidagi “Buyurtmalar” bo‘limida mijoz ma’lumotlari va holatni yangilash mumkin

## 7. Ishga tushirish
```bash
npm run dev
```
`http://localhost:3000` manzilini oching.
- `/register` — ro‘yxatdan o‘tish
- `/login` — kirish
- `/admin` — admin panel (admin roliga ega bo‘lgandagina)
- Bosh sahifa — mahsulotlar va buyurtma berish

## 8. Xavfsizlik
- Supabase service-role key brauzerga berilmaydi va `.env.local`ga qo‘yilgan bo‘lsa ham `NEXT_PUBLIC_` bilan boshlanmasligi shart.
- RLS siyosatlari mijozga mahsulotlarni o‘zgartirishni taqiqlaydi.
- Buyurtma narxi serverda bazadagi mahsulot narxidan olinadi, brauzer yuborgan narxga ishonilmaydi.
- Ishga chiqarishdan oldin rate limit, spam himoyasi, maxfiylik siyosati, zaxira nusxalari va server loglarini qo‘shing.
