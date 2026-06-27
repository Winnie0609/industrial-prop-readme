# IndustrialProp

An AI-powered platform for discovering industrial property (warehouses, factories, industrial land) across Malaysia's Klang Valley. The project consists of a public-facing marketing/search site and an internal admin dashboard, built on a single Next.js App Router codebase.

> Built as a full-stack product: this repo is the frontend + BFF layer, talking to a separate API backend.

## Highlights

- **Landy AI**: a streaming conversational assistant that searches live listings by spec, location, logistics and budget (not just keywords), with visible tool-use, reasoning trail, follow-up suggestions and inline lead capture.
- **Server-rendered, SEO-first**: App Router with per-page metadata, Open Graph, JSON-LD and ISR caching.
- **Multilingual**: English, Bahasa Malaysia, Simplified & Traditional Chinese via `next-intl`.
- **BFF proxy pattern**: Next route handlers proxy the upstream API, keeping auth tokens server-side and avoiding CORS.

## Features

### User site

- **Search & browse**: filter by category, offer type (sale/rent), location, price and size; semantic free-text query; sorting and pagination.
- **Listing detail**: full specs, image gallery, interactive map (MapLibre GL) and agent contact.
- **Landy AI assistant** (`/landy-ai`): natural-language property search with streamed responses, live result cards, and one-click handoff to a human agent.
- **Blog**: articles with rich content rendering.
- **Content pages**: About, Contact, Buyers Guide, List Your Property, plus legal pages.
- **Lead capture**: contact and enquiry forms across the site.

### Admin dashboard (`/admin`)

- **Auth**: cookie-based token login, protected routes.
- **Overview**: listing / blog / lead counts and recent activity.
- **Listings**: full CRUD with a multi-step form, image upload + drag-to-reorder, and a detail sheet.
- **Blog posts**: CRUD with a BlockNote rich-text editor and a live preview route.
- **Leads**: table of incoming enquiries.
- **Conversations**: logs of Landy AI chat sessions.
- **Settings**: admin profile management.

## Tech stack

| Area        | Choice                                                        |
| ----------- | ------------------------------------------------------------- |
| Framework   | Next.js 16 (App Router), React 19, TypeScript                 |
| Styling     | Tailwind CSS v4, shadcn/ui (Radix primitives), `lucide-react` |
| Data        | TanStack Query, TanStack Table                                |
| Forms       | React Hook Form + Zod                                         |
| i18n        | next-intl (4 locales)                                         |
| Editor      | BlockNote                                                     |
| Maps        | MapLibre GL                                                   |
| Animation   | Framer Motion                                                 |
| Interaction | dnd-kit (drag & drop), Sonner (toasts)                        |
| Tooling     | ESLint, Prettier, pnpm                                        |

## Architecture

```
Browser ──▶ Next.js route handlers (/api/*)  ──▶  Upstream API backend
            · attach auth token from cookie
            · same-origin proxy (no CORS)
            · stream AI chat responses
```

- Server Components fetch directly from the upstream API with the request's auth cookie.
- Client Components call the local `/api/*` proxy, which injects the token and forwards the request.
- Admin auth uses an `admin_token` cookie; 401s clear the session and redirect to login.
