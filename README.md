# Maryam Cosmetics & Beauty — E-commerce Platform

A complete, production-ready online store for **Maryam Cosmetics & Beauty** (Pakistan, PKR, Cash on Delivery) with a full admin dashboard, WhatsApp ordering and email notifications.

* **Storefront:** premium blush/beige design, mobile-first, product pages, cart drawer, COD checkout, order confirmation, order tracking, contact form, newsletter, SEO (sitemap, robots, structured data).
* **Admin dashboard (`/admin`):** products (unlimited), images, variants, inventory, categories, orders, customers, reviews, coupons, homepage banners/content, featured products, shipping, store settings, newsletter subscribers, messages.
* **Backend:** Next.js (App Router, server actions + route handlers) · TypeScript · Tailwind CSS · PostgreSQL via Drizzle ORM. Works with local Postgres, **Supabase**, Neon, Railway, Render, etc.
* **No dependency on Arena AI or any other builder** — it is a standard Node/Next.js project.

---

## 1. Quick start (local)

Requirements: **Node.js 20+** and a **PostgreSQL** database.

```bash
npm install
cp .env.example .env          # then edit DATABASE_URL (and optional email / storage keys)
npm run setup                 # creates all tables + loads the 15 sample products, categories & banners
npm run dev                   # http://localhost:3000
```

Create your admin login (pick ONE):

* open **http://localhost:3000/admin/setup** and create the owner account (one-time page, disabled afterwards), **or**
* run `npm run admin:create -- you@example.com "a-strong-password"`, **or**
* set `ADMIN_EMAIL` + `ADMIN_PASSWORD` in `.env` — the account is created on first start.

Then sign in at **/admin/login**.

Production build locally: `npm run build && npm start`.

---

## 2. Environment variables

See `.env.example` for the full annotated list. Never commit your real `.env`.

| Variable | Required | Purpose |
|---|---|---|
| `DATABASE_URL` | ✅ | PostgreSQL connection string |
| `DATABASE_SSL` | hosted DBs | `true` for Supabase / Neon / Railway |
| `NEXT_PUBLIC_SITE_URL` | recommended | Public URL (sitemap, SEO tags, email links) |
| `ADMIN_SETUP_KEY` | optional | Protects `/admin/setup` so nobody else can claim the first admin account |
| `ADMIN_EMAIL` / `ADMIN_PASSWORD` | optional | Auto-create the first admin (alternative to `/admin/setup`) |
| `SESSION_SECRET` | optional | Admin session signing key (random one is generated & stored in DB if empty) |
| `OWNER_EMAIL` | optional | Fallback owner address (the one in Admin → Store Settings wins) |
| `RESEND_API_KEY`, `EMAIL_FROM` | for email | **Resend** email provider |
| `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS` | for email | **SMTP** provider (e.g. Gmail App Password) |
| `CLOUDINARY_*` or `SUPABASE_URL` + `SUPABASE_SERVICE_ROLE_KEY` + `SUPABASE_STORAGE_BUCKET` | optional | Cloud image storage (default: PostgreSQL) |

All secrets are read **server-side only** (`process.env`) — nothing private is ever sent to the browser.

---

## 3. Deploying (Vercel + Postgres — recommended)

> **Using a Hostinger domain?** See **[DEPLOYMENT.md](DEPLOYMENT.md)** for the exact DNS records (A / CNAME), Hostinger hPanel steps, the Vercel route and a full Hostinger KVM VPS walkthrough.

> Hostinger **shared/cloud** plans cannot run this app (no Node.js, no PostgreSQL). Use Vercel (+ Neon/Supabase) or a Hostinger **KVM VPS**.

1. **Database** — create a free project at https://supabase.com or https://neon.tech. Copy the connection string. Use it as `DATABASE_URL` and set `DATABASE_SSL=true`.
2. **Create the tables** — from your computer, with `DATABASE_URL` in `.env`:
   ```bash
   npm install
   npm run setup        # creates tables (drizzle-kit push) + sample data
   ```
3. **Import this repo** in https://vercel.com. Framework preset: **Next.js**.
4. **Add environment variables** in Vercel → Settings → Environment Variables (at least `DATABASE_URL`, `DATABASE_SSL`, `NEXT_PUBLIC_SITE_URL`).
5. **Deploy**, then open `https://your-site/admin/setup` and create your admin account.
6. Add your custom domain in Vercel and update `NEXT_PUBLIC_SITE_URL`.

See the full **DEPLOYMENT.md** for Hostinger DNS, email setup, WhatsApp, and troubleshooting.

---

## License

Private project for Maryam Cosmetics & Beauty.
