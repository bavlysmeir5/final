# FORMA — bilingual fashion storefront

A mobile-first Arabic/English storefront for clothing, shoes, and jackets. The Vite/React app runs as a self-contained local demo; the Laravel 11 API source is in `backend/`.

## Run the storefront

Requirements: Node.js 20.19+ or 22.12+.

```powershell
npm install
npm start
```

Open the local URL printed by Vite (normally `http://localhost:5173`). The demo includes responsive English/Arabic with automatic LTR/RTL, catalog filtering, product details, size/color selection, a persistent local cart and wishlist, sign-in demo, checkout, order history, and a product-management demo form. Demo credentials are not sent to a server and demo orders are not real purchases.

## Laravel API setup

Requirements: PHP 8.2+, Composer, and the PHP SQLite extension (or MySQL 8+).

```powershell
Set-Location backend
Copy-Item .env.example .env
composer install
php artisan key:generate
New-Item -ItemType File -Force database\database.sqlite
php artisan migrate --seed
php artisan serve
```

The API is available at `http://127.0.0.1:8000/api/v1`. It provides localized catalog responses (`Accept-Language: en` or `ar`), Sanctum SPA authentication, customer cart/wishlist/address/order routes, and role-protected admin product/order routes. To connect the storefront catalog and account/cart/checkout actions, run Vite at `http://127.0.0.1:5173`, set `VITE_API_BASE_URL=http://127.0.0.1:8000` in a frontend `.env.local`, and restart Vite. Keep the frontend and API on the same host (`127.0.0.1`) for local Sanctum cookies. Without that setting, the storefront uses its local demo catalog and browser storage.

Create an account through the API, then promote that account from the backend folder with `php artisan forma:make-admin you@example.com`. No admin password or production credentials are seeded.

For production, use a shared registrable domain, enable HTTPS, set `SESSION_DOMAIN` and `SANCTUM_STATEFUL_DOMAINS`, and restrict `CORS_ALLOWED_ORIGINS` to the exact SPA origin. The local `.env.example` values are for local development only. No payment gateway is connected; API orders are created in `pending` state.

## Useful commands

```powershell
# Frontend
npm run build
npm run lint

# Backend (from backend/)
php artisan test
```

## Structure

```text
backend/                 Laravel REST API, migrations, seeders, feature tests
src/
  api/                   Axios API client and typed endpoint helpers
  data/products.ts       Bilingual catalog demo data
  i18n/locales/          en.json and ar.json
  store/                 Persisted Zustand cart/account demo state
  App.tsx                Storefront, detail modal, cart, checkout and account views
```
