# GLAMOUR LUXE — Bangkok Model Directory

A full-stack starter project for a premium black-and-champagne-gold model directory.
Includes public directory, search/filtering, profiles, member registration/login, favorites,
model dashboard, verification submissions, VIP subscriptions, Stripe Checkout integration,
payment history, and an admin dashboard.

## Important status

This is a runnable **starter implementation**, not a deployed production service. It uses demo profiles and needs your own hosting, database, domain, email provider, and Stripe credentials before it can accept real users/payments. Do not upload real identity documents until you have configured secure production storage, access controls, retention/deletion policies, and reviewed local legal requirements.

Only adults (18+) may register as models. Verification is reviewed by an admin. A VIP badge is a paid/admin-assigned visibility tier, not a claim that a person is identity-verified. These states are separate in the data model.

## Requirements

- Node.js 20+
- PostgreSQL 15+ (or another PostgreSQL-compatible service)
- Stripe account for real payments (optional during local development)

## Quick start

1. Install dependencies:
   ```bash
   npm install
   ```
2. Copy `.env.example` to `.env` and fill in values.
3. Create the database and run migrations:
   ```bash
   npm run db:generate
   npm run db:migrate
   npm run db:seed
   ```
4. Start the app:
   ```bash
   npm run dev
   ```
5. Open `http://localhost:5173`.

The backend runs on `http://localhost:4000`. Vite proxies `/api` requests to it.

## Demo admin

The seed script creates the admin from `ADMIN_EMAIL` and `ADMIN_PASSWORD` in `.env`.
Change the example password before running seed. The app never hardcodes a production admin password.

## Stripe setup

Set `STRIPE_SECRET_KEY` and `STRIPE_WEBHOOK_SECRET` in `.env`, then configure a webhook endpoint to:
`https://YOUR_DOMAIN/api/payments/webhook`
Subscribe to `checkout.session.completed` and `checkout.session.expired`.
For local webhook testing, use Stripe CLI and forward events to `localhost:4000/api/payments/webhook`.

VIP checkout uses Stripe Checkout. VIP is activated only after the verified webhook event is received. Do not activate VIP based only on the browser redirect.

## Verification uploads

The demo accepts a verification submission with a short description and an optional document upload. Uploaded files are stored in a local private folder outside `client/public`, are never served as static assets, and can only be downloaded by an authorized admin endpoint. For production, replace local storage with a private object-storage bucket, virus scanning, encryption at rest, strict retention, deletion workflow, and audit logging. Do not collect more identity data than you need.

## Production checklist

- Use HTTPS and a managed PostgreSQL database.
- Set a long random `JWT_SECRET` and strong admin credentials.
- Configure trusted origins, rate limiting, security monitoring, backups and restore tests.
- Configure Stripe webhook signing secret and test payment/refund flows.
- Configure transactional email for verification, password reset and payment receipts.
- Add terms, privacy notice, consent, cookie policy, refund/cancellation rules, and a data deletion process.
- Review Thailand and applicable international privacy, consumer, tax, and business requirements with qualified counsel.
- Add a human review process for identity documents and content reports.
- Never expose Stripe secret keys, JWT secret, or identity documents in frontend code or public folders.

## Features

- Black/champagne-gold responsive UI
- Model directory, text search, filters, profile pages
- Registration, login and role-based access
- Model profile editing and favorite models
- Verification application flow (`PENDING`, `VERIFIED`, `REJECTED`)
- VIP subscriptions (1, 3, 6 months) through Stripe Checkout
- Admin VIP assignment and verification review
- Admin model/user/booking/payment dashboard
- Booking inquiry form for professional modeling work
- Audit-friendly payment status model

## Not included yet

The project does not include a production email sender, password-reset emails, automated ID-document authenticity checks, tax invoices, payouts to models, or a fully deployed live environment. Those require your service credentials, business decisions, and provider setup.


## Replit deployment notes

- For development, run `npm run dev`; the Vite client binds to `0.0.0.0` and the API binds to `0.0.0.0`.
- For a production deployment, build with `npm run build` and start with `npm start`. The Express server serves the built frontend from `dist`.
- Configure `DATABASE_URL` using Replit PostgreSQL, set a long random `JWT_SECRET`, and set `NODE_ENV=production` for deployment.
- Initialize a new/empty database with `npm run db:generate` followed by `npm run db:push`; seed demo data only if desired.
- Stripe payments remain disabled until valid Stripe secret and webhook keys are configured. Never put secrets in source files or send them in chat.
