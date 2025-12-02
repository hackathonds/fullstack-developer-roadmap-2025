# Projects — Specifications & Acceptance Criteria

This file outlines the main projects, required features and acceptance criteria.

Frontend:
1. Personal Portfolio Website
   - Responsive, accessible, SEO, dark/light themes, deployed (Netlify/GitHub Pages).
   - Acceptance: Lighthouse score >= 85, WCAG AA checks pass.

2. Interactive Weather Dashboard
   - OpenWeatherMap integration, forecast charts, geolocation.
   - Acceptance: Works offline for cached cities, shows 5-day forecast.

3. Task Management App (React + TypeScript)
   - Drag & drop, user auth, backend sync, SignalR for real-time.
   - Acceptance: CRUD tasks, board persistence, real-time updates.

4. E-commerce Catalog
   - Product listing, filter, search, cart (client-side), routes, SSR option.
   - Acceptance: Product filtering and cart persist across reloads.

Backend:
1. RESTful Blog API (ASP.NET Core + EF Core)
   - CRUD posts, comments, pagination, swagger, unit tests.
   - Acceptance: Post endpoints have unit and integration tests.

2. User Authentication Service
   - JWT, refresh tokens, role-based access, password hashing.
   - Acceptance: Login and token refresh flows covered by tests.

3. File Upload Service
   - Upload to Azure Blob, signed URLs, virus-scan step (placeholder).
   - Acceptance: Upload/download works, blobs stored with proper metadata.

Full Stack:
1. Social Media Platform (MVP)
   - Profiles, follow, posts, likes, comments, real-time notifications.
   - Acceptance: Core feed functionality with production-ready DB design.

2. E-commerce Full App (Next.js + .NET)
   - Payments (Stripe sandbox), order flow, admin panel.
   - Acceptance: Checkout flow and payment testing in sandbox mode.

Each project folder should include:
- spec.md with requirements
- starter template or docker-compose
- test checklist & acceptance criteria