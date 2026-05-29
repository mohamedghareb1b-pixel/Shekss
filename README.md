# شيكس — Shekss Project Structure

منصة عروض وخصومات حصرية في مصر.

---

## 🗂️ هيكل المشروع

```
shekss/
│
├── index.html                  # ملف HTML الرئيسي (SPA)
│
├── styles/
│   └── main.css                # كل CSS الموقع (variables → components → pages → responsive)
│
├── services/
│   ├── supabase.js             # Supabase client initialization
│   ├── data.js                 # Categories, EGYPT_GOVS, CAT_LABELS (static data)
│   └── seo.js                  # Dynamic SEO: titles, descriptions, schemas, updateSEO()
│
├── scripts/
│   ├── utils.js                # toast, copyCoupon, countdown engine, slideImg, imageUpload
│   ├── deals.js                # App state, dealCard(), buildQuery(), home & deals pages
│   ├── expired.js              # Expired deals page: filters, pagination, load more
│   ├── coupons-blog.js         # loadCoupons(), cpCard(), loadBlog(), blogCard(), sendMsg()
│   └── nav-location.js         # showPage(), toggleMob(), location, FAQ, login tabs, showApp()
│
├── components/
│   ├── navbar.html             # Navbar reference (مُدمج في index.html)
│   ├── footer.html             # Footer reference (مُدمج في index.html)
│   ├── deal-card.html          # Deal card template reference
│   └── filters.html            # Filters components reference
│
├── pages/
│   └── index.html              # Page registry & documentation
│
└── assets/                     # Static assets (images, icons, etc.)
```

---

## 📄 الصفحات

| الصفحة | ID | URL |
|--------|-----|-----|
| الرئيسية | `home` | `/` |
| العروض | `deals` | `/?p=deals` |
| عروض منتهية | `expired` | `/?p=expired-deals` |
| كوبونات | `coupons` | `/?p=coupons` |
| المدونة | `blog` | `/?p=blog` |
| تواصل معنا | `contact` | `/?p=contact` |
| من نحن | `about` | `/?p=about` |
| الأسئلة الشائعة | `faq` | `/?p=faq` |
| تسجيل الدخول | `login` | `/?p=login` |

---

## 🔗 تسلسل تحميل الـ Scripts

```
1. supabase.js       ← Supabase SDK (CDN) ثم client init
2. data.js           ← CAT_LABELS, ONLINE_CATS, OFFLINE_CATS, EGYPT_GOVS
3. seo.js            ← PAGE_TITLES, PAGE_SCHEMAS, updateSEO(), URL_PAGE_MAP
4. utils.js          ← toast, copyCoupon, countdown, slideImg
5. deals.js          ← State variables, dealCard, buildQuery, home/deals logic
6. expired.js        ← Expired page logic (يعتمد على deals.js)
7. coupons-blog.js   ← Coupons, blog, contact logic
8. nav-location.js   ← showPage, toggleMob, routing, showApp, initHomePage
9. inline script     ← showApp() — يُشغّل التطبيق
```

---

## 🔧 Services

### `supabase.js`
- يُنشئ `sb` (Supabase client) كـ global variable
- كل DB calls تستخدم `sb`

### `data.js`
- `CAT_LABELS` — object لعرض أسماء الأقسام
- `ONLINE_CATS` — مصفوفة أقسام الأونلاين مع subcategories
- `OFFLINE_CATS` — مصفوفة أقسام جمبي مع subcategories
- `EGYPT_GOVS` — المحافظات والمراكز

### `seo.js`
- `updateSEO(page)` — تحديث title + meta + canonical + OG + schema + history
- `URL_PAGE_MAP` — reverse mapping لـ URL params → page IDs

---

## 🧩 Components

ملفات الـ components هي **reference files** فقط — الكود الفعلي مُدمج في `index.html` لأن المشروع SPA بدون build tool. عند التطوير المستقبلي إلى React/Vue/Next.js، هذه الملفات تُوضح بنية كل component.

---

## 🚀 للإضافة مستقبلاً

المشروع جاهز للتوسع في:

- **Authentication** — Supabase Auth scaffold موجود في `services/supabase.js`
- **Merchant Dashboard** — صفحة جديدة `page-merchant`
- **Admin Dashboard** — صفحة جديدة `page-admin`
- **Ratings** — إضافة column في `deals` table + UI component
- **Notifications** — Supabase Realtime + notification component
- **Store Pages** — صفحات `/?p=store&id=X`

---

## 📐 CSS Variables

```css
--blue, --blue-dark, --blue-light
--orange, --orange-dark, --orange-light
--green, --green-light
--purple, --purple-light
--red, --red-light
--gray-50/100/200/400/600/800
--white, --radius, --shadow
```

---

## 📊 Database Tables (Supabase)

| Table | الوصف |
|-------|-------|
| `deals` | العروض (status: active/hot/expired, is_offline, category...) |
| `coupons` | الكوبونات (code, site_name, discount, expires_at...) |
| `blogs` | مقالات المدونة (title, content, status: published...) |
| `messages` | رسائل تواصل معنا |
| `partnerships` | طلبات الشراكة (legacy — الصفحة محذوفة من الـ nav) |
