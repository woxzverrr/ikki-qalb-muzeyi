# Supabase sozlash

Supabase bepul loyihasini yarating va `Project URL` hamda `anon/public` kalitini oling.
`service_role` kalitini hech qachon sayt fayliga yoki GitHub'ga joylamang.

## 1. Ma’lumotlar bazasi

Supabase Dashboard → **SQL Editor** → **New query**:

1. `schema.sql` faylining hamma matnini nusxalang.
2. Query oynasiga qo‘yib, **Run** tugmasini bosing.

## 2. Administrator hisobi

1. **Authentication → Users → Add user** orqali admin uchun email va kuchli parol bilan hisob yarating.
2. SQL Editor’da quyidagi so‘rovni ishlating (emailni o‘zingiznikiga almashtiring):

```sql
update public.profiles
set role = 'admin'
where id = (
  select id from auth.users where lower(email) = lower('admin-email@example.com')
);
```

Administrator saytda **Administrator** turini tanlab, shu email va parol bilan kiradi.

## 3. Tashrifchilarni boshqarish Edge Function

Supabase Dashboard → **Edge Functions** → **Deploy a new function**:

1. `functions/manage-visitors/index.ts` kodini `manage-visitors` nomli function’ga joylang.
2. Function sozlamasida **Verify JWT** yoqilgan bo‘lsin.
3. Dashboard → **Edge Functions → Secrets** bo‘limida `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` qiymatlari borligini tekshiring. `SERVICE_ROLE_KEY` faqat server secret bo‘lib qolishi shart.

## 4. Sayt kalitlari

`supabase-config.js` faylidagi bo‘sh `url` va `anonKey` qiymatlarini Supabase Project URL va **anon/public** kaliti bilan to‘ldiring. Faqat anon kalitni brauzerda ishlatish mumkin; RLS siyosatlari ma’lumotni himoya qiladi.

Supabase Dashboard → **Authentication → URL Configuration** sahifasida Site URL sifatida `https://woxzverrr.github.io/ikki-qalb-muzeyi/` kiriting va shu manzilni Redirect URLs ro‘yxatiga qo‘shing. Kirish sahifasidagi **Parolni unutdingizmi?** havolasi emailga tiklash xatini yuboradi; email havolasi saytda yangi parol o‘rnatish formasini ochadi. Bu imkoniyat email bilan ochilgan hisoblar uchun. Login orqali ochilgan tashrifchi hisoblarida email bog‘lanmagan, ular parolni administratordan yangilatishi kerak.

Fayllarni GitHub Pages’ga yuborgach, admin media yuklaydi va tashrifchi hisoblarini yaratadi. Media yopiq Storage bucket’da saqlanadi; faqat tizimga kirgan muzey a’zolari ko‘ra oladi. Sayt media ko‘rsatishda 24 soatlik vaqtinchalik havolalardan foydalanadi.

Avvalgi versiyadagi brauzer-local hisoblar va media fayllar serverga avtomatik ko‘chmaydi. Yangi visitor hisoblarini admin paneldan qayta yarating va kerakli rasmlar/videolarni serverga qayta yuklang.
