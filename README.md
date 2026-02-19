# Discovery Consulting / Agrar Malaka Markazi Web Platform

A Laravel 9-based web application with:
- a **PHP backend (main part)** for content management and routing,
- a **Blade-powered frontend** for the public website and admin CRUD pages,
- a MySQL-compatible relational schema for dynamic content.

The project appears to power an institutional website (news, about, cooperation partners, leadership, council) with an admin panel for managing content.

---

## 1) Tech Stack

### Backend (Main PHP Part)
- **PHP 8+**
- **Laravel 9**
- **Eloquent ORM** for data access
- **Form Request validation** for CRUD inputs
- **Laravel Breeze auth scaffolding** (login/register/profile)

### Frontend
- **Blade templates**
- **Bootstrap 5** (CDN + local styles)
- **Vite tooling** (configured in `package.json`)
- **Static assets** in `public/` (CSS/JS/libs/images)

---

## 2) What the Application Does

The application is split into two functional areas:

### A. Public Website
Public routes render content pages:
- `/` – news/home
- `/aboutlink` – about center
- `/xamkorliklink` – international cooperation
- `/raxbariyatlink` – leadership
- `/kengashlink` – scientific council
- `/apparatlink` – currently mapped to leadership method
- `/contact` – contact page

These pages are served by `PrintContoller` methods that read data from Eloquent models and return Blade views.

### B. Admin / Content Management
Resource controllers provide CRUD for:
- News (`/news`)
- About (`/abouts`)
- Cooperation (`/xamkorlik`)
- Leadership (`/raxbariyat`)
- Council (`/kengash`)

Profile/dashboard routes are protected by auth middleware.

---

## 3) Project Structure (Important Parts)

```text
app/
  Http/
    Controllers/
      PrintContoller.php      # public content rendering
      NewsController.php      # CRUD: news
      AboutController.php     # CRUD: about
      XamkorController.php    # CRUD: cooperation
      RaxbarController.php    # CRUD: leadership
      KengashController.php   # CRUD: council
    Requests/                 # validation classes
  Models/
    News.php
    About.php
    Cooparation.php
    Raxbariyat.php
    Kengash.php

routes/
  web.php                     # all web routes (public + admin resources)
  auth.php                    # Breeze auth routes

resources/
  views/
    layouts/template.blade.php  # public site layout
    welcome.blade.php           # homepage/news rendering
    about.blade.php
    cooparation.blade.php
    raxbariyat.blade.php
    kengash.blade.php
    crud/...                    # admin CRUD blade pages
  css/app.css
  js/app.js

database/
  migrations/                  # schema definitions

public/
  css/, js/, lib/, img/        # production-facing static assets
```

---

## 4) Data Model

Core content tables:
- `news` → `title`, `text`, `img` (handled by model fillable)
- `abouts` → `title`, `text`
- `cooparations` → `title`, `img`, `text`
- `raxbariyats` → `fullName`, `img`, `job`
- `kengashes` → `fullName`, `job`, timestamps

Authentication/system tables are also present (users, password resets, failed jobs, personal access tokens).

---

## 5) Local Setup

## Prerequisites
- PHP 8.0+
- Composer
- Node.js 18+ (recommended)
- npm
- MySQL/MariaDB

### Installation
```bash
git clone <your-repo-url>
cd Discovery-consulting
composer install
npm install
cp .env.example .env
php artisan key:generate
```

### Database
1. Update `.env` with DB credentials.
2. Run migrations:
```bash
php artisan migrate
```

(Optional) If you want initial data, import SQL dump if your team uses it:
```bash
# example only
# mysql -u <user> -p <db_name> < database/SQL/db_bimm.sql
```

### Run in Development
Start backend:
```bash
php artisan serve
```

Start frontend dev server:
```bash
npm run dev
```

Open:
- App: `http://127.0.0.1:8000`

---

## 6) Frontend Notes

- Main public layout is `resources/views/layouts/template.blade.php`.
- Public pages are section-based Blade templates extending that layout.
- UI relies heavily on static assets under `public/lib`, `public/css`, `public/js`, and `public/img`.
- Vite is configured, but the public pages currently use many direct static includes and CDN links (Bootstrap, icons, animation libs).

---

## 7) Backend Notes (PHP-focused)

- Route definitions are centralized in `routes/web.php`.
- Controllers implement standard Laravel resource flow (`index/create/store/edit/update/destroy`).
- Validation classes exist in `app/Http/Requests/*Request.php` and are used in `store()` methods.
- File uploads for image fields are moved directly to `public/img` via `move('img/', $fileName)`.
- Models define `$fillable` arrays for mass assignment.

---

## 8) Useful Commands

```bash
# list all routes
php artisan route:list

# run tests
php artisan test

# code style
./vendor/bin/pint

# production frontend build
npm run build
```

---

## 9) Operational Recommendations

To improve maintainability and production readiness:

1. Add route middleware/authorization for admin resource routes.
2. Standardize file upload handling using Laravel storage (`Storage`) and symbolic links.
3. Ensure all migrations have correct rollback logic.
4. Add seeders/factories for reproducible environments.
5. Add automated tests for CRUD controllers and public page rendering.
6. Consolidate frontend asset strategy (Vite-first or static-first) to avoid duplication.

---

## 10) License

This project is based on Laravel and uses the MIT license ecosystem unless your organization defines a stricter internal license.
