<div align="center">

# 🔒 Nadia Elsayed — Showcase Repository

### This repo documents a real client project. The source code is private and stays that way.

![repo](https://img.shields.io/badge/repo-showcase%20only-critical)
![code](https://img.shields.io/badge/source-private-red)
![platform](https://img.shields.io/badge/platform-iOS%20%7C%20Android%20%7C%20Web%20%7C%20API-blue)

<img src="assets/status-light.svg" width="230" alt="Blinking light: source code is private">

*Curious about the implementation? [Get in touch](https://github.com/youssefemadeldn).*

</div>

---

**Chef Nadia El-Sayed's official cooking platform** — a multi-part product built around her YouTube
content (video playlists, Shorts, and community posts), browsable on mobile and web, curated through an
admin console, and answerable in her own Egyptian-Arabic voice by a retrieval-augmented AI assistant.

> This repository is a **monorepo of four independently-governed projects**, not one application. Each
> project has its own stack, its own contributor rules (`<project>/CLAUDE.md`), and its own lifecycle.
> They are wired together by a **shared PostgreSQL database** and a **shared Redis instance**.

![platform](https://img.shields.io/badge/platform-iOS%20%7C%20Android%20%7C%20Web%20%7C%20API-blue)
![backend](https://img.shields.io/badge/backend-.NET%2010-512BD4)
![mobile](https://img.shields.io/badge/mobile-Flutter%203.44.8-02569B)
![ai](https://img.shields.io/badge/ai-Python%203.12%20%2F%20FastAPI-3776AB)
![admin](https://img.shields.io/badge/admin-Next.js%2016-000000)
![license](https://img.shields.io/badge/license-proprietary-lightgrey)

---

## Table of Contents

- [Overview](#overview)
- [Screens](#screens)
- [Repository layout](#repository-layout)
- [Projects](#projects)
  - [Backend (`backend/`)](#backend-backend)
  - [Mobile (`mobile/`)](#mobile-mobile)
  - [AI (`ai/`)](#ai-ai)
  - [Admin (`admin/`)](#admin-admin)
- [Shared infrastructure](#shared-infrastructure)
- [Tech stack](#tech-stack)
- [Getting started](#getting-started)
- [Environment variables reference](#environment-variables-reference)
- [API overview](#api-overview)
- [Testing](#testing)
- [Build & deployment](#build--deployment)
- [Documentation (`doc/`)](#documentation-doc)
- [Contributing](#contributing)
- [Known limitations / TODO](#known-limitations--todo)
- [License](#license)

---

## Overview

The platform is organised around content that originates on Chef Nadia's YouTube channel and is then
**curated, enriched, and republished** through the platform's own model:

- **Browsing** — Categories, Playlists, Videos, Shorts, and Posts. Fully usable as a guest; no login
  required to browse. Login is only needed to comment and to sync favorites across devices.
- **Playlists** — independent of YouTube's own playlists; an admin builds each one by hand (name +
  cover image) and orders videos manually.
- **Videos** — imported from an auto-fetched list of the channel's uploads. Import copies only
  title / thumbnail / YouTube URL / duration; the app description, ingredient list, instruction steps,
  difficulty, meal type, budget level, prep/cook time, tags, and related recipes are all written by an
  admin afterwards. A video stays an unpublished draft until an admin publishes it.
- **Ingredients & recipe scaling** — each ingredient carries a machine-usable numeric amount alongside
  its as-written display text and links optionally to a shared master ingredient list. The API returns
  base amounts + serving count; the client does the scaling math.
- **Meal planner** — weekly plans generated from category/tag preferences and prep/cook-time filters.
- **Ingredient matching** — "what can I cook with what I have" two-stage matching against recipes.
- **AI assistant (Nadia AI)** — a RAG service that answers cooking questions from structured recipe
  data, phrased in Egyptian colloquial Arabic in Chef Nadia's speaking voice. See
  `ai/PHASE_1_AI_PLAN.md`.

Payments happen on YouTube (Channel Membership) — there is no in-app purchase. Arabic is the primary
language; English is secondary. Authoritative product behaviour lives in
`doc/context/product-spec.md`.

---

## Screens

These are the **mobile design prototype** — the interactive Claude Design canvases in
`doc/design/ui-design/`, captured per screen into
`doc/design/screenshots/`. They are **not** screenshots of the running
Flutter app; the app itself has not been screenshotted running yet (see
[Known limitations / TODO](#known-limitations--todo)). The UI is Arabic-first (RTL); the phone bezel
is part of the design mock.

### Auth

<table>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/auth-01-splash.png" width="180" alt="Auth — Splash"><br><sub>Splash</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/auth-02-login.png" width="180" alt="Auth — Login"><br><sub>Login</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/auth-03-register.png" width="180" alt="Auth — Register"><br><sub>Register</sub></td>
</tr>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/auth-04-verify-otp.png" width="180" alt="Auth — Verify OTP"><br><sub>Verify OTP</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/auth-05-forgot.png" width="180" alt="Auth — Forgot password"><br><sub>Forgot password</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/auth-06-reset.png" width="180" alt="Auth — Reset password"><br><sub>Reset password</sub></td>
</tr>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/auth-07-profile.png" width="180" alt="Auth — Profile"><br><sub>Profile</sub></td>
</tr>
</table>

### Browse

<table>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/browse-01-home.png" width="180" alt="Browse — Home"><br><sub>Home</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/browse-02-search.png" width="180" alt="Browse — Search & filters"><br><sub>Search &amp; filters</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/browse-03-category.png" width="180" alt="Browse — Categories"><br><sub>Categories</sub></td>
</tr>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/browse-04-playlist.png" width="180" alt="Browse — Playlist"><br><sub>Playlist</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/browse-05-video.png" width="180" alt="Browse — Video detail"><br><sub>Video detail</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/browse-06-short.png" width="180" alt="Browse — Short"><br><sub>Short</sub></td>
</tr>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/browse-07-posts.png" width="180" alt="Browse — Community posts"><br><sub>Community posts</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/browse-08-favorites.png" width="180" alt="Browse — Favorites"><br><sub>Favorites</sub></td>
</tr>
</table>

### Meal Planner

<table>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/planner-01-list.png" width="180" alt="Meal Planner — Plans list"><br><sub>Plans list</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/planner-02-create.png" width="180" alt="Meal Planner — Create plan"><br><sub>Create plan</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/planner-03-view.png" width="180" alt="Meal Planner — View plan"><br><sub>View plan</sub></td>
</tr>
</table>

### Cooking Mode, Nadia AI & tools

<table>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/cooking-01-step.png" width="180" alt="Cooking Mode — Step with timer"><br><sub>Cooking Mode</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/nadia-ai-01-tab.png" width="180" alt="Nadia AI — General chat"><br><sub>Nadia AI — general</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/nadia-ai-02-cooking.png" width="180" alt="Nadia AI — From cooking mode"><br><sub>Nadia AI — from cooking</sub></td>
</tr>
<tr>
<td align="center" width="200"><img src="doc/design/screenshots/ingredient-matching-01.png" width="180" alt="Ingredient Matching — Pick ingredients"><br><sub>Ingredient Matching</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/pick-for-me-01.png" width="180" alt="Pick For Me — Preferences"><br><sub>Pick For Me</sub></td>
<td align="center" width="200"><img src="doc/design/screenshots/shopping-list-01.png" width="180" alt="Shopping List"><br><sub>Shopping List</sub></td>
</tr>
</table>

---

## Repository layout

```
Nadia Elsayed/
├── backend/        # ASP.NET Core / .NET 10 — the core REST API and content database owner
├── mobile/         # Flutter / Dart — Chef Nadia's consumer cooking app (iOS + Android)
├── ai/             # Python / FastAPI + Celery — "Nadia AI" RAG service over the shared Postgres
├── admin/          # Next.js 16 — the admin console for curating all content (+ an AI chat page)
├── doc/            # Cross-project docs: product spec, technical decisions, design system, handoffs
├── start-dev.ps1   # Windows dev orchestrator — starts AI + backend + admin together
├── CLAUDE.md       # Root contributor guide — cross-project boundary rules
└── .gitignore
```

`backend/`, `ai/`, and `admin/` each ship a `Dockerfile` + `.dockerignore` for deployment (see
[Build & deployment](#build--deployment)); `mobile/` is built with the Flutter toolchain, not
containerised.

Each project's own `CLAUDE.md` is the authority for everything inside its folder. The root `CLAUDE.md`
only sets the rules that hold across all of them — most importantly, that a backend migration touching
a content table is a **cross-project change** because the AI service attaches triggers to those tables.

---

## Projects

### Backend (`backend/`)

The REST API and the owner of the shared content database (`public` schema).

- **Stack:** ASP.NET Core on **.NET 10** (`net10.0`), EF Core with **Npgsql / PostgreSQL**, ASP.NET
  Core Identity + **JWT bearer** auth, Redis (`StackExchange.Redis` + `HybridCache` + Data Protection
  key-ring), Serilog, OpenTelemetry (traces + metrics, OTLP), API versioning (`Asp.Versioning`),
  NSwag/OpenAPI, FluentValidation, AutoMapper, Polly, Cloudflare R2 via `AWSSDK.S3`, Bogus (dev seed
  data).
- **Architecture:** strict one-way layered split across physical projects
  (`Data → Repository → Services → WebAPI`), enforced by `NetArchTest` architecture tests. Specification
  pattern for queries, `ServiceResult<T>` response envelope, single `ExceptionMiddleware` error
  boundary, tag-based cache invalidation, EN/AR localisation from day one.
- **Solution:** `NadiaElsayed.slnx` — projects `NadiaElsayed.Data`, `.Repository`, `.Services`,
  `.WebAPI`, `.WebAPI.Tests`. Startup project for `dotnet ef` and `dotnet run` is `NadiaElsayed.WebAPI`
  (dev HTTP port **5032**).
- **Feature controllers:** `Auth`, `AdminAuth`, `AdminDashboard`, `AdminUsers`, `Categories`,
  `Playlists`, `Videos`, `Shorts`, `Posts`, `Comments`, `Favorites`, `Tags`, `Ingredients`,
  `IngredientMatching`, `MealPlans`, `Search`, `YouTubeImport`.
- **Dev seeding:** `IdentitySeeder` (SuperAdmin), `DemoDataSeeder` / `MockDataSeeder` (hand-authored
  rows), and `RealContentSeeder` — an opt-in, existence-guarded, prod-safe seeder that fills an empty
  database from the live YouTube channel via the same catalog client the admin import screens use.
  Enable it for a single boot with `SEED_REAL_CONTENT=true`; it runs AI-ingestion trigger checks and
  logs. Decisions: `doc/context/real-content-seeding-decisions.md`
  and `doc/context/real-content-playlist-category-map.md`.
- **Rules:** `backend/CLAUDE.md` plus `.claude/rules/dotnet_scaffold_prompt.md`
  and `.claude/rules/dotnet_feature_prompt.md`.

```
backend/
├── NadiaElsayed.Data/          # EF Core entities, IEntityTypeConfiguration<T>, DbContext, migrations
├── NadiaElsayed.Repository/    # Generic repository + UnitOfWork + Specification pattern
├── NadiaElsayed.Services/      # Business logic, DTOs, FluentValidation validators, localisation resources
├── NadiaElsayed.WebAPI/        # Controllers, middleware, DI wiring, Program.cs, appsettings.json
│   ├── Controllers/            # Thin controllers — parse input, call one service, wrap result
│   ├── Extensions/             # AddCaching / AddIdentityService / AddRateLimiting / AddSwagger / ...
│   ├── Middleware/             # ExceptionMiddleware, SecurityHeadersMiddleware
│   └── Helpers/                # ServiceRegistration, seeders, DotEnvLoader, ConnectionStringResolver
├── NadiaElsayed.WebAPI.Tests/  # Unit / Integration / Architecture / Common test areas
├── deploy/                     # Migration scripts + grant_nadia_ai.sql (restricted AI DB role)
├── Dockerfile                  # Multi-stage; listens :8080, non-root, ASPNETCORE_ENVIRONMENT=Production
└── restart-api.ps1             # Dev helper: stop the :5032 listener, `dotnet run` again, hit /health
                                #   (-Rebuild compiles instead of --no-build)
```

### Mobile (`mobile/`)

Chef Nadia's consumer cooking app for iOS and Android.

- **Stack:** Flutter / Dart (SDK `^3.12.0`; FVM pins **3.44.8** — `mobile/.fvmrc`), `flutter_bloc`
  (Cubit), `get_it` + `injectable` DI, `dio` networking, `go_router`, `dartz` (`Either` error
  handling), `flutter_secure_storage`, `shared_preferences`, `flutter_screenutil` (390×844 canvas),
  `easy_localization` (Arabic-primary, English fallback), `cached_network_image`, `connectivity_plus`,
  **`youtube_player_iframe`** + **`wakelock_plus`** (in-app video + cooking mode), **`firebase_core`** +
  **`firebase_messaging`** (push notifications).
- **Architecture:** Clean Architecture, strict feature-first. Infrastructure in `lib/core/`, product
  code in `lib/features/<feature>/` with `data / domain / presentation` layers. `AuthInterceptor`
  handles token injection + silent refresh; `ApiManager` is the sole error boundary. Dark-only theme.
  Bundled fonts (Great Vibes, IBM Plex Sans Arabic, IBM Plex Mono).
- **App identity:** package `nadia_elsayed`, version `1.0.0+1`, `publish_to: none`.
- **Features present:**
  - Full `data / domain / presentation` (cubits + model-parsing tests): `auth` (login, register,
    confirm-email, forgot/reset password, profile), `browse` (home, search, category, playlist, video,
    short, posts, **cooking mode**), `comments`, `favorites`, `meal_planner`, `recipe_discovery`
    (ingredient matching + "pick for me").
  - Presentation only (not yet API-wired): `nadia_ai`, `shopping_list`.
  - Bottom navigation: Home / Search / Nadia AI / Shopping List / Account.
- **Rules:** `mobile/CLAUDE.md` plus `.claude/rules/flutter_scaffold_prompt.md`,
  `.claude/rules/flutter_feature_prompt.md`, and `mobile/.claude/skills/` (design-asset / core-widget /
  screen-building playbooks).

```
mobile/lib/
├── core/
│   ├── constants/     # AppConstants, ApiConstants (env-aware baseUrl), AppSpacing, AppImages, AppIcons
│   ├── di/            # injection_container.dart (@InjectableInit), register_module.dart
│   ├── network/       # ApiManager, ApiResult, Failure hierarchy, DioFactory, interceptors/
│   ├── router/        # AppRouter (GoRouter), AppRoutes, args/ (typed navigation args)
│   ├── session/       # session state + bootstrap
│   ├── push/          # Firebase Cloud Messaging wiring
│   ├── storage/       # SecureStorageHelper
│   ├── theme/         # AppColors, AppTextStyles, AppTheme.darkTheme, ... (one file per token group)
│   ├── localization/  # easy_localization setup
│   ├── helpers/       # DialogHelper, SnackBarHelper, BottomSheetHelper, formatters
│   ├── demo/          # app_demo_seed.dart — offline demo data for presentation-only features
│   └── widgets/       # AppLoadingIndicator, EmptyStateWidget, ErrorStateWidget
└── features/
    ├── auth/  browse/  comments/  favorites/  meal_planner/  recipe_discovery/   # data/domain/presentation
    └── nadia_ai/  shopping_list/                                                 # presentation only
```

### AI (`ai/`)

"Nadia AI" — an Egyptian-dialect RAG service over the shared backend Postgres.

- **Stack:** Python `>=3.12`, managed with **uv** (`uv.lock` committed). Core: `pydantic` /
  `pydantic-settings`, `sqlalchemy` 2.x, `alembic`, `psycopg[binary]`, Typer CLI. Optional dependency
  groups: `pipeline` (FastAPI + Uvicorn, Celery + Redis, `asyncpg`, `llama-index`, `instructor` +
  `openai`, `pgvector`, `structlog`), `transcripts` (`google-api-python-client` + OAuth,
  `faster-whisper`, `yt-dlp`), `eval` (`ragas`).
- **Ownership boundary (enforced by DB grants):** this service **owns the `ai` schema** (full DML +
  Alembic migrations) and has **`SELECT`-only** access to the backend's `public."Video"`,
  `public."VideoIngredient"`, and `public."AspNetUsers"`. It attaches two `SECURITY DEFINER` triggers
  to `public` content tables so a newly published recipe becomes answerable with no restart. It must
  connect as the restricted role `nadia_ai` (`backend/deploy/grant_nadia_ai.sql`), never `postgres`.
- **Hard product requirement:** every stored recipe step, tip, ingredient name, and every generated
  answer is in Egyptian colloquial Arabic (العامية المصرية) in Chef Nadia's voice — enforced at
  extraction (Pydantic validators), generation (style exemplars), and evaluation (dialect metric).
- **Surface:** `GET /health` (liveness + capabilities + schema/outbox health), `POST /ask`
  (Tier-1 structured-fact answers spoken in Nadia's voice). Maintenance CLI: `python -m app.cli`.
- **Migrations:** `migrations/versions/0001` … `0010` (extensions, content tables, voice tables,
  learning tables, outbox + triggers, embeddings, seeds, YouTube quota usage).
- **Rules:** `ai/CLAUDE.md`; architecture of record in
  `ai/PHASE_1_AI_PLAN.md`, with
  `ai/PHASE_2_AI_PLAN.md` and
  `ai/PHASE_2_REMAINING_AND_IMPROVEMENTS.md`.

```
ai/
├── app/
│   ├── main.py         # FastAPI app factory, /health, lifespan schema check
│   ├── config.py       # pydantic-settings — the ONLY place env vars are read
│   ├── worker.py       # Celery app + beat schedule
│   ├── api/            # Route handlers (ask.py) — thin, delegate to retrieval
│   ├── db/             # SQLAlchemy models (ai schema), session, checks, seeds/, export/
│   ├── schemas/        # Pydantic v2 extraction contracts + dialect validators
│   ├── transcripts/    # YouTube captions OAuth client, Whisper fallback, quality scoring
│   ├── ingestion/      # LISTEN worker, outbox sweeper, extraction, chunking, embedding
│   ├── style/          # Style-exemplar mining + retrieval (the voice layer)
│   ├── retrieval/      # tier1_sql.py, router.py, generate.py, format_facts.py
│   └── youtube/        # YouTube Data API catalog client
├── migrations/                    # Alembic — owns the `ai` schema ONLY
├── scripts/                       # db_status, list_recipes, seed_* probes, resolve_dev_database_url.py
├── tests/                         # unit/ + integration/ (integration touches the shared Postgres)
├── Dockerfile                     # One image, 3 stages: final (API) / worker / beat; listens :8000
├── docker-compose.pgvector.yml    # Local pgvector helper only — not a deploy compose
└── docker-entrypoint.sh           # Runs `alembic upgrade head` when RUN_MIGRATIONS_ON_STARTUP=true
```

### Admin (`admin/`)

The web console an admin uses to curate every piece of content — and, more recently, to run an **AI
chat / knowledge-review workspace** against the Nadia AI service.

- **Stack:** Next.js **16.3.1** (App Router) on React **19.2.8**, TypeScript 5, **pnpm 11.21.0**.
  TanStack Query 5 + Table 8, shadcn/ui 4 + `@base-ui/react` + Tailwind CSS v4 + `tw-animate-css`,
  `react-hook-form` + `zod`, `jose` + `server-only` (JWT cookie handling), Recharts,
  `@dnd-kit/*` (playlist ordering), `react-easy-crop`, Framer Motion, Sonner, next-themes,
  `lucide-react`.
- **Architecture:** BFF pattern — Route Handlers under `src/app/api/**` are the only code that talks
  to a backend; `src/lib/api/backend-client.ts` reads `API_URL` (the ASP.NET backend) and
  `src/lib/ai/admin-fetch.ts` reads `AI_BASE_URL` + `AI_ADMIN_TOKEN` (the Nadia AI service). Neither
  origin reaches the browser bundle. Cookie-based session with transparent refresh rotation in
  `api/auth/me`; `middleware.ts` does a presence-only session check and redirects to `/login`.
  `GET /api/health` is a process-liveness route (no backend call) for the container healthcheck.
- **Dashboard sections:** content — `categories`, `playlists`, `posts`, `shorts`, `videos` (each with
  a detail route), plus `content`, `moderation`, `notifications`, `settings`; `ai` (chat, feedback,
  knowledge, step-review, unresolved); `youtube-import`; `admin-users`; `login` / `change-password`.
- **Rules:** `admin/CLAUDE.md` → `admin/AGENTS.md` (the
  Next.js 16 breaking-changes guide). `admin/README.md` is the default `create-next-app` stub.

```
admin/src/
├── app/
│   ├── (dashboard)/           # Authenticated shell + (content-sections)/ + ai/ route groups
│   ├── api/                   # Route Handlers proxying backend + AI (auth, videos, playlists, ai/*, ...)
│   ├── login/ change-password/
│   └── layout.tsx  globals.css
├── components/                # dashboard/ (incl. ai/), forms/, ui/ (shadcn)
├── lib/                       # api/ (backend-client), ai/ (admin-fetch), auth/, queries/, schemas/
├── hooks/
└── middleware.ts              # route guard
```

---

## Shared infrastructure

| Resource | Backend uses | AI uses |
|---|---|---|
| **PostgreSQL** (one database) | Owns the `public` schema (EF Core migrations) | Owns the `ai` schema (Alembic); `SELECT`-only on `public` content tables + triggers; connects as role `nadia_ai` |
| **Redis** (one instance) | Logical **db 0** — `HybridCache` L2 + Data Protection key ring | A separate logical db (`AI_CELERY_DB_INDEX`, never 0) — Celery broker/result backend |
| **`.env`** at the repo root | Read by `DotEnvLoader` in `Program.cs` | Read by `app/config.py` (`pydantic-settings`) |

Because both halves share the database and neither ORM models the other's half, **a backend migration
that renames, drops, or recreates a content table silently breaks AI ingestion.** Treat content-table
schema changes as cross-project — check `ai/PHASE_1_AI_PLAN.md` first.

The repo-root `.env` is gitignored and never committed. There is **no root `docker-compose.yml`** —
PostgreSQL and Redis are expected to be running already (locally or as managed services).

---

## Tech stack

### Backend — .NET 10 (`backend/NadiaElsayed.WebAPI/NadiaElsayed.WebAPI.csproj`)

| Group | Packages (version) |
|---|---|
| Framework | `net10.0` |
| Data | `Npgsql.EntityFrameworkCore.PostgreSQL` 10.0.3, `Microsoft.EntityFrameworkCore.Design` 10.0.11 |
| Auth | `Microsoft.AspNetCore.Authentication.JwtBearer` 10.0.11, `System.IdentityModel.Tokens.Jwt` 8.22.0, ASP.NET Core Identity |
| Caching / resilience | `Microsoft.Extensions.Caching.StackExchangeRedis` 10.0.11, `Microsoft.Extensions.Caching.Hybrid` 10.9.0, `Microsoft.AspNetCore.DataProtection.StackExchangeRedis` 10.0.11, `Polly` 8.7.0 |
| Validation / mapping | `FluentValidation.AspNetCore` 11.3.1, `AutoMapper` 16.1.1 |
| API surface | `Asp.Versioning.Mvc` / `.Mvc.ApiExplorer` 10.2.1, `Microsoft.AspNetCore.OpenApi` 10.0.11, `NSwag.AspNetCore` 14.7.1 |
| Logging / observability | `Serilog.AspNetCore` 10.0.0, `OpenTelemetry.*` 1.17.0 (ASP.NET Core / HTTP / EF Core / Runtime + OTLP exporter) |
| Storage | `AWSSDK.S3` 4.0.102.1 (Cloudflare R2) |
| Testing / seed | `Bogus` 35.6.5, xUnit, Moq, FluentAssertions, `Microsoft.AspNetCore.Mvc.Testing`, EF Core SQLite, Testcontainers, `NetArchTest.Rules`, NBomber |

### Mobile — Flutter (`mobile/pubspec.yaml`)

| Group | Packages (constraint) |
|---|---|
| SDK | Dart/Flutter `sdk: ^3.12.0` (FVM `3.44.8`) |
| Networking | `dio` ^5.11.0, `pretty_dio_logger` ^1.4.0, `connectivity_plus` ^7.3.1 |
| State | `flutter_bloc` ^9.1.1, `equatable` ^2.1.0 |
| DI | `get_it` ^9.2.1, `injectable` ^3.0.0 (`injectable_generator` ^3.1.1, `build_runner` ^2.15.1) |
| Routing | `go_router` ^17.5.0 |
| FP / storage | `dartz` ^0.10.1, `flutter_secure_storage` ^11.0.0, `shared_preferences` ^2.5.5 |
| Media / UI | `youtube_player_iframe` ^6.0.2, `wakelock_plus` ^1.8.0, `flutter_screenutil` ^5.9.3, `flutter_svg` ^2.3.0, `cached_network_image` ^3.4.1, `shimmer` ^3.0.0, `cupertino_icons` ^1.0.8 |
| Push | `firebase_core` ^4.1.0, `firebase_messaging` ^16.0.1 |
| i18n / misc | `easy_localization` ^3.0.8, `intl` ^0.20.2, `url_launcher` ^6.3.2 |
| Dev | `flutter_lints` ^6.0.0, `flutter_launcher_icons` ^0.14.4 |

### AI — Python (`ai/pyproject.toml`)

| Group | Packages (constraint) |
|---|---|
| Runtime | Python `>=3.12`, managed with **uv** |
| Core | `pydantic` >=2.9, `pydantic-settings` >=2.5, `sqlalchemy` >=2.0.36, `alembic` >=1.14, `psycopg[binary]` >=3.2, `typer` >=0.12 |
| `pipeline` extra | `fastapi` >=0.115, `uvicorn[standard]` >=0.32, `celery` >=5.4, `redis` >=5.2, `asyncpg` >=0.30, `llama-index-core` >=0.12, `llama-index-vector-stores-postgres` >=0.3, `instructor` >=1.6, `openai` >=1.54, `pgvector` >=0.3.6, `structlog` >=24.4 |
| `transcripts` extra | `google-api-python-client` >=2.149, `google-auth` >=2.35, `google-auth-oauthlib` >=1.2, `faster-whisper` >=1.0.3, `yt-dlp` >=2024.8 |
| `eval` extra | `ragas` >=0.2.6, `pyyaml` >=6 |
| Dev group | `pytest` >=8.3, `pytest-asyncio` >=0.24, `ruff` >=0.8, `mypy` >=1.13 |

### Admin — Next.js (`admin/package.json`)

| Group | Packages (version) |
|---|---|
| Framework | `next` 16.3.1, `react` / `react-dom` 19.2.8, `typescript` ^5, package manager `pnpm@11.21.0` |
| Data | `@tanstack/react-query` ^5.101.4, `@tanstack/react-table` ^8.21.3 |
| Forms / validation | `react-hook-form` ^7.85.0, `@hookform/resolvers` ^5.8.0, `zod` ^3.25.76 |
| Auth | `jose` ^6.2.8, `server-only` ^0.0.1 |
| UI | `tailwindcss` ^4, `tw-animate-css` ^1.4.0, `shadcn` ^4.18.0, `@base-ui/react` ^1.7.0, `lucide-react` ^1.31.0, `class-variance-authority`, `tailwind-merge`, `clsx`, `cmdk`, `sonner` ^2.0.8, `next-themes` ^0.4.6, `framer-motion` ^13.1.0, `@dnd-kit/*`, `react-easy-crop` ^6.2.3, `recharts` ^3.8.0, `react-day-picker` ^10, `date-fns` ^4.4.0 |
| Dev | `eslint` ^9 + `eslint-config-next` 16.3.1, `@tailwindcss/postcss` ^4 |

---

## Getting started

Clone the repo, then set up whichever project(s) you need. All four run against the same local
PostgreSQL + Redis.

### Prerequisites

| Tool | Version | Needed by |
|---|---|---|
| .NET SDK | 10.x | backend |
| PostgreSQL | 14+ (with `pgvector` if running AI) | backend, ai |
| Redis | 6+ | backend, ai |
| Flutter SDK | 3.44.8 (via FVM; Dart `^3.12.0`) | mobile |
| Python | 3.12+ | ai |
| uv | latest | ai |
| Node.js | 20+ | admin |
| pnpm | 11.21.0 | admin |

> `pgvector` and a `pgvector`-enabled Postgres can be spun up locally with
> `docker compose -f ai/docker-compose.pgvector.yml up` — a convenience for AI dev only, not a
> deployment artifact.

### Environment setup

- **Repo-root `.env`** (gitignored) — shared by the backend (`DotEnvLoader`) and the AI service
  (`app/config.py`). Holds `DATABASE_URL`, `REDIS_CONNECTION`, and the YouTube keys at minimum.
- **Backend secrets** — locally via `dotnet user-secrets`; deployed via environment variables. Never
  commit real values to `appsettings*.json` (it holds shape only).
- **Admin** — copy `admin/.env.example` to `admin/.env`; set `API_URL` (the running backend, e.g.
  `http://localhost:5032/api/v1.0`) and, for the AI pages, `AI_BASE_URL` (e.g.
  `http://localhost:8100`) + `AI_ADMIN_TOKEN`. All server-only, never `NEXT_PUBLIC_`.
- **Mobile** — no `.env`; environment is selected at compile time with
  `--dart-define=ENVIRONMENT=dev|prod` (URLs live in `lib/core/constants/api_constants.dart`).

Full key list: [Environment variables reference](#environment-variables-reference).

### Run everything at once (Windows)

```powershell
./start-dev.ps1                 # stops stale listeners, then starts AI :8100, backend :5032, admin :3000
./start-dev.ps1 -WithCelery     # also start a Celery worker (spends OpenAI quota on the ingestion outbox)
./start-dev.ps1 -SeedRealContent # pass SEED_REAL_CONTENT=true to the backend for this boot
./start-dev.ps1 -Rebuild        # `dotnet run` with a build (default is --no-build)
./start-dev.ps1 -ApiOnly        # restart only the backend, leave AI + admin running
```

Requires the AI virtualenv (`ai/.venv`, created by `uv sync --extra pipeline`), `admin/node_modules`
(`pnpm install`), and `pnpm` on `PATH`. Postgres and Redis must already be reachable.

### Backend

```bash
cd backend
dotnet restore
dotnet build

# Local secrets (example)
dotnet user-secrets init --project NadiaElsayed.WebAPI
dotnet user-secrets set "ConnectionStrings:NadiaElsayedDB" "Host=localhost;Port=5432;Database=nadiaelsayed;Username=postgres;Password=..." --project NadiaElsayed.WebAPI

# EF Core migrations (Data owns the model; WebAPI is the startup project)
dotnet ef database update --project NadiaElsayed.Data --startup-project NadiaElsayed.WebAPI

# Run (Development auto-migrates + seeds; port 5032 on the http profile)
dotnet run --project NadiaElsayed.WebAPI
# or, on Windows, the restart helper:
./restart-api.ps1            # add -Rebuild to compile instead of --no-build

# Tests
dotnet test
```

Swagger UI is served in Development only. Health check: `GET /health`.

### Mobile

```bash
cd mobile
fvm install                                                # provisions Flutter 3.44.8 (from .fvmrc)
fvm flutter pub get
fvm dart run build_runner build --delete-conflicting-outputs   # generate DI config

fvm flutter run                                   # dev environment (default)
fvm flutter run --dart-define=ENVIRONMENT=prod    # prod environment

fvm flutter analyze
fvm flutter test

# Release builds
fvm flutter build apk --dart-define=ENVIRONMENT=prod
fvm flutter build ios --dart-define=ENVIRONMENT=prod
```

(Drop `fvm ` if you run a system Flutter pinned to the same version. The repo `.vscode/launch.json`
has ready-made `dev` / `prod` run configs.)

### AI

```bash
cd ai
uv sync                          # core deps
uv sync --extra pipeline         # + FastAPI / Celery / retrieval stack
uv sync --extra transcripts      # + YouTube captions / Whisper

# Alembic migrations (owns the `ai` schema only)
uv run alembic upgrade head

# API
uv run uvicorn app.main:app --reload --port 8100

# Celery worker + beat (ingestion / embedding)
uv run celery -A app.worker.celery_app worker --loglevel=info
uv run celery -A app.worker.celery_app beat   --loglevel=info

# Maintenance CLI
uv run python -m app.cli --help

# Lint / types / tests
uv run ruff check . && uv run ruff format --check .
uv run mypy app
uv run pytest                    # excludes tests/eval (Ragas — slow, costs tokens)
```

### Admin

```bash
cd admin
pnpm install
cp .env.example .env             # set API_URL (backend) + AI_BASE_URL / AI_ADMIN_TOKEN (AI service)

pnpm dev                         # http://localhost:3000
pnpm build && pnpm start         # production
pnpm lint
```

---

## Environment variables reference

Values are never committed. Names and purposes only.

### Repo-root `.env` — shared (backend + AI)

| Key | Purpose | Required? |
|---|---|---|
| `DATABASE_URL` | PostgreSQL connection (libpq URI). Backend fallback + AI primary. | Yes |
| `REDIS_CONNECTION` | Redis host. Backend cache + AI Celery broker (AI appends its own db index). | Yes (AI worker; backend caching) |
| `YOUTUBE_API_KEY` / `YouTube__ApiKey` | YouTube Data API v3 — channel catalog / import metadata. | Yes (import + AI catalog) |
| `YOUTUBE_CHANNEL_ID` / `YouTube__ChannelId` | The channel to import from. | Yes (import + AI catalog) |

### Backend — `appsettings.json` keys (set via `dotnet user-secrets` / env vars)

| Key | Purpose | Required? |
|---|---|---|
| `ConnectionStrings:NadiaElsayedDB` | PostgreSQL connection string (else `DATABASE_URL` fallback). | Yes |
| `Redis:ConnectionString` | Redis connection (else `REDIS_CONNECTION` fallback). | Yes |
| `Jwt:Key` | JWT signing key. | Yes |
| `Jwt:Issuer` / `Jwt:Audience` / `Jwt:AdminIssuer` / `Jwt:AdminAudience` | Token issuer/audience (user + admin). | Yes (defaults present) |
| `Jwt:AccessTokenExpirationMinutes` / `Jwt:RefreshTokenExpirationDays` | Token lifetimes. | No (defaults 60 / 7) |
| `R2:AccountId` / `R2:AccessKey` / `R2:SecretKey` / `R2:BucketName` / `R2:PublicBaseUrl` | Cloudflare R2 image storage. | Yes (image upload) |
| `Email:SmtpHost` / `SmtpPort` / `SmtpUsername` / `SmtpPassword` / `SenderEmail` / `SenderName` / `EnableSsl` | SMTP for OTP / confirmation emails. | Yes (email flows) |
| `SuperAdmin:Email` / `Password` / `FirstName` / `LastName` | Bootstrap SuperAdmin (seeded every environment). | Yes |
| `Google:ClientId` | Google social-login audience. | Yes (Google sign-in) |
| `Apple:ClientId` | Apple social-login audience. | Yes (Apple sign-in) |
| `YouTube:ApiKey` / `YouTube:ChannelId` | YouTube import. | Yes (import) |
| `Cors:AllowedOrigins` | Allowed browser origins (array). Empty ⇒ permissive in Development only. | No |
| `RateLimiting:*` | Auth / general / ingredient-matching limiter permits + windows. Non-secret. | No (defaults present) |
| `OpenTelemetry:OtlpEndpoint` | OTLP collector endpoint. SDK no-ops if empty. | No |
| `SEED_REAL_CONTENT` | When `true`, `RealContentSeeder` imports real channel content on this boot. Opt-in, then flip back off. | No (default false) |
| `RUN_MIGRATIONS_ON_STARTUP` | Deployed environments only — when `true`, applies pending EF Core migrations before Kestrel binds. Set on **one** instance. Development always auto-migrates regardless. | No (default false) |

### AI — `app/config.py` (repo-root `.env`)

| Key | Purpose | Required? |
|---|---|---|
| `DATABASE_URL` | Same Postgres as the backend, but as the `nadia_ai` role. | Yes |
| `REDIS_CONNECTION` | Same Redis host as the backend. | Yes (Celery worker) |
| `AI_CELERY_DB_INDEX` | Redis logical db for Celery. **Must not be 0.** | No (default 2) |
| `OPENAI_API_KEY` | Embeddings + generation. Service runs *degraded* without it. | Yes (answer/embed) |
| `AI_EMBEDDING_MODEL` | Embedding model. | No (default `text-embedding-3-small`) |
| `AI_EXTRACTION_MODEL` / `AI_GENERATION_MODEL` | LLM models for extraction / generation. | No (default `gpt-4o-mini`) |
| `AI_RETRIEVAL_SCORE_FLOOR` | Below this score the answer degrades to "I don't know". | No (default 0.35) |
| `AI_ADMIN_TOKEN` | Shared secret the admin console presents to reach AI admin endpoints. | Yes (admin AI pages) |
| `YOUTUBE_OAUTH_CLIENT_ID` / `_CLIENT_SECRET` / `_REFRESH_TOKEN` | Channel-owner OAuth for `captions.download` (an API key cannot download captions). | Yes (transcript fetch) |
| `YOUTUBE_DAILY_QUOTA_BUDGET` | YouTube Data API daily quota cap. | No (default 5000) |
| `WHISPER_MODEL` | `faster-whisper` model for the transcript fallback. | No (default `large-v3`) |
| `SSL_CERT_FILE` | TLS trust bundle override (Windows / AV interception). | No |
| `RUN_MIGRATIONS_ON_STARTUP` | When `true`, `docker-entrypoint.sh` runs `alembic upgrade head` before the role command. Set on the **API** role only, never worker/beat. | No (default false) |

### Admin — `admin/.env`

| Key | Purpose | Required? |
|---|---|---|
| `API_URL` | Server-only ASP.NET backend origin + version prefix (e.g. `http://localhost:5032/api/v1.0`). Never `NEXT_PUBLIC_`. | Yes |
| `AI_BASE_URL` | Server-only Nadia AI service origin (e.g. `http://localhost:8100`). | Yes (AI pages) |
| `AI_ADMIN_TOKEN` | Bearer token for AI admin endpoints (matches the AI service's `AI_ADMIN_TOKEN`). | Yes (AI pages) |
| `NEXT_PUBLIC_APP_URL` | Public site origin used for metadata / absolute URLs. | No |

### Mobile

No env files. Environment is chosen at build time: `--dart-define=ENVIRONMENT=dev|prod` (URLs in
`lib/core/constants/api_constants.dart`; documented for VS Code `launch.json` and Android Studio run
configurations in `mobile/CLAUDE.md`).

---

## API overview

- **Base URL:** `{{host}}/api/v{version}` — current version `v1.0`. Also `GET /health` and `/error`
  (unversioned).
- **Auth:** JWT bearer. Separate user and admin token issuers/audiences. Refresh-token rotation;
  OTP-hashed email confirmation and password reset. Social login (Google, Apple). Auth and OTP
  endpoints run under a stricter fixed-window rate-limit policy.
- **Response envelope:** every endpoint returns `ServiceResult<T>` —
  `{ success, message, errorCode, data, statusCode, timestamp, errors }`. Clients branch on the
  stable `errorCode`, never on the (localised) `message`. Validation failures return `422`.
- **Localisation:** `Accept-Language: en` or `ar` selects content fields and messages.
- **Endpoint groups:** `auth`, `admin/auth`, `admin/dashboard`, `admin/users`, `categories`,
  `playlists`, `videos`, `shorts`, `posts`, `comments`, `favorites`, `tags`, `ingredients`,
  `ingredient-matching`, `meal-plans`, `search`, `youtube-import`.

### Nadia AI service (separate process)

- `GET /health` — liveness plus a capabilities block (`ingest` / `embed` / `answer` and what is
  blocking each), schema revision, and outbox backlog.
- `POST /ask` — body `{ query, ingredient_hint?, target_servings? }`; returns an answer (Egyptian
  Arabic), the tier used, a dialect score, and — when the query matches more than one recipe — a list
  of `candidates` to disambiguate on the next turn.

---

## Testing

| Project | Framework | Location | Run |
|---|---|---|---|
| Backend | xUnit, Moq, FluentAssertions, `WebApplicationFactory`, `NetArchTest`, NBomber | `backend/NadiaElsayed.WebAPI.Tests/` — `Unit/`, `Integration/{Controllers,Database}/`, `Architecture/`, `Common/` | `dotnet test` |
| Mobile | `flutter_test` | `mobile/test/` — boot smoke, network-envelope, and per-feature model-parsing tests (auth, browse, comments, favorites, meal_planner, recipe_discovery) | `fvm flutter test` |
| AI | pytest (+ `pytest-asyncio`) | `ai/tests/unit/`, `ai/tests/integration/` (real Postgres, rolled-back transactions); `ai/tests/eval/` is the Ragas suite, excluded from the default loop | `uv run pytest` |
| Admin | — | — | No test setup detected in this repo. |

Backend architecture tests mechanically enforce the layering and DTO/entity-separation rules. The AI
integration suite runs against the shared backend Postgres (its triggers cannot exist without the
backend's own tables) — never Testcontainers/SQLite for those paths.

---

## Build & deployment

Deployment infra (host, provider, proxy, credentials) is intentionally **not documented here** — this
repo has a public remote. What follows is the deployment *model*; environment-specific detail lives in
private ops notes.

### Container images

`backend/`, `ai/`, and `admin/` each ship a multi-stage `Dockerfile` + `.dockerignore`. Configuration
is entirely environment-variable driven; no secrets are baked into an image. `mobile/` is not
containerised — it ships as an APK / IPA from `flutter build`.

| Service | Build context | Container port | Runs as | Healthcheck | Migrations |
|---|---|---|---|---|---|
| backend | `./backend` | 8080 | non-root `$APP_UID`, `ASPNETCORE_ENVIRONMENT=Production`, forwarded headers on | `GET /health` | `RUN_MIGRATIONS_ON_STARTUP=true` on exactly one instance; otherwise a `dotnet ef database update` deploy step |
| ai | `./ai` | 8000 | non-root `app`; one image, three build-stage targets (`final` = API / `worker` / `beat`) selected per role | `GET /health` (`docker-healthcheck.py`) | `RUN_MIGRATIONS_ON_STARTUP=true` on the API role only → `alembic upgrade head`; never on worker/beat |
| admin | `./admin` | 3000 | non-root `nextjs`, Next.js standalone `node server.js` (`output: "standalone"`) | `GET /api/health` | n/a |

The backend test project is excluded from its image; the AI image bundles `alembic.ini` /
`migrations/` / `scripts/` but strips `tests/`. The AI service is meant to stay internal — the .NET
backend is the public API and the only client that should reach it (`Ai__BaseUrl` / `Ai__AdminToken`).

`ai/docker-compose.pgvector.yml` is a **local pgvector helper only** — there is no root/deploy
`docker-compose.yml`.

### CI/CD

> **Not detected in this repo.** There is no `.github/workflows/`, `azure-pipelines.yml`,
> `Jenkinsfile`, `.gitlab-ci.yml`, `bitrise.yml`, or `codemagic.yaml`. Image builds, tests,
> migrations, and deploys are triggered manually against the `main` branch. The backend rules note
> that the vulnerability scan (`dotnet list package --vulnerable --include-transitive`) is a manual
> pre-release step "until a CI pipeline exists."

---

## Documentation (`doc/`)

| Path | Contents |
|---|---|
| `doc/context/product-spec.md` | Authoritative product behaviour ("the what") — features, entities, API contract at a business level. |
| `doc/context/backend-technical-decisions.md` | Backend-specific technical decisions + local setup. |
| `doc/context/mobile-technical-decisions.md` | Mobile-specific technical decisions. |
| `doc/context/ai-technical-decisions.md` | AI-specific technical decisions. |
| `doc/context/deployment-docker-decisions.md` | Docker image / deployment decisions. |
| `doc/context/real-content-seeding-decisions.md`, `real-content-playlist-category-map.md` | The `RealContentSeeder` design + the 42-playlist → category map. |
| `doc/context/*transcript*.md`, `nadia new requirment.md`, `001-*-new-requirements-handoff.md` | Source discovery material and new-requirements handoffs. |
| `doc/design/design-system/` | Design tokens (colors, fonts, spacing) and assets consumed by the mobile theme. |
| `doc/design/ui-design/` | UI design references (`.dc.html` screen flows). |
| `doc/handoffs/001…012/` | Numbered per-sprint handoff notes (backend/mobile foundations, auth, content sprint, posts, review/rules hardening, mobile design tokens, Phase 2, design-conformance fixes, real-content seeding). |
| `ai/PHASE_1_AI_PLAN.md`, `PHASE_2_AI_PLAN.md`, `PHASE_2_REMAINING_AND_IMPROVEMENTS.md` | The AI architecture of record and its phase-2 scope. |
| `admin-updates-for-phase2-backend.md` (repo root) | Admin changes required for the Phase 2 backend. |

---

## Contributing

Conventions inferred from git history (no `CONTRIBUTING.md` present — treat as a suggestion until one
is added):

- **Branch:** work targets `main`. The GitHub remote is `youssefemadeldn/Nadia-Elsayed`.
- **Commits:** Conventional Commits — `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, usually scoped
  (`feat(chat): …`, `feat(seeder): …`) and sometimes phase-tagged.
- **Per-project rules are binding.** Read the target project's `CLAUDE.md` (and its `.claude/rules/`)
  before changing code in that folder. Never carry one project's conventions into another.
- **Cross-project changes:** a schema change to a backend content table must be checked against
  `ai/PHASE_1_AI_PLAN.md` — the AI service reads those tables and attaches triggers to them.
- **Handoffs:** significant chunks of work are captured as a numbered folder under `doc/handoffs/`.

---

## Known limitations / TODO

Built from in-repo notes, rule files, and code scan:

- **No CI pipeline.** Deployment Dockerfiles exist for backend / ai / admin, but there is no
  `docker-compose.yml` and no automated build/test/deploy pipeline — vulnerability scans, tests, and
  image builds are triggered manually.
- **AI retrieval & generation depend on credentials.** Per `ai/PHASE_1_AI_PLAN.md`, the ingestion
  backbone, resolution, transcript scoring and `/health` are implemented, but retrieval/generation
  need `OPENAI_API_KEY` and channel-owner OAuth for caption download. `/ask` serves Tier-1 structured
  facts only until those are set.
- **Admin has no automated tests** and still ships the default `create-next-app` README; parts of the
  UI use mock data.
- **Mobile has two presentation-only features** (`nadia_ai`, `shopping_list`) backed by
  `lib/core/demo/app_demo_seed.dart`; the rest are API-wired.
- **Data Protection key-ring** is persisted to Redis, but the backend rules flag that no cross-request
  DP-token consumer exists yet — revisit before adding email-change links or "remember me".
- **Recipe-scaling behaviour by ingredient category is intentionally not built** — the master
  ingredient list stores a scaling-category *label* only; the actual math is a culinary-team decision
  (product spec, "Ingredient master list").
- **`backend/dump.rdb`** (a Redis dump) is tracked in the working tree though `.rdb` is gitignored —
  a local artifact, not part of the app.
- Inline `TODO`/`FIXME` markers are minimal; most open work is tracked in `doc/handoffs/` and the
  per-project `CLAUDE.md` files.

---

## License

**No `LICENSE` file is present at the repository root or in any project.** Treat the code as
proprietary / unlicensed — all rights reserved by the project owner. The mobile package is explicitly
marked `publish_to: 'none'`.
