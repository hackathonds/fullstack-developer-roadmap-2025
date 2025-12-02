# Phase 4 — Backend API Development (Weeks 13–18)

## Objective
Design and implement robust backend services using ASP.NET Core 8+, EF Core, authentication (JWT/OAuth2), testing, and API documentation.

## Outcomes
- Clean architecture project layout.
- Secure Web APIs with token-based auth.
- Unit and integration test coverage for controllers/services.

## Week-by-week Plan
Week 13 — API Basics & Architecture
- Project structure (Domain, Application, Infrastructure, API).
- Controller patterns, DTOs, AutoMapper usage.

Week 14 — Data & EF Core
- Code-first migrations, relationships, transactions.
- Performance: N+1 problems, query optimization.

Week 15 — Security & Auth
- JWT tokens, refresh token patterns.
- Integrate Identity or external providers (Azure AD, Auth0).

Week 16 — File Handling & Storage
- Streaming uploads, Azure Blob Storage integration.
- Signed URLs and storage security.

Week 17 — Testing & CI
- xUnit, Moq, integration tests with WebApplicationFactory.
- Add server-side test coverage and CI steps.

Week 18 — API Docs & Observability
- Swagger/OpenAPI tooling.
- Logging (Serilog) and basic metrics (Application Insights).

## Projects
- RESTful Blog API (posts, comments, tags, pagination).
- Auth Service (login, refresh, role-based authorization).
- File Upload Service (Azure Blob + metadata).

## Acceptance Checklist
- [ ] API has Swagger and documented endpoints.
- [ ] Authentication flows implemented and tested.
- [ ] CI pipeline runs backend tests.

## Resources (Focused)
- ASP.NET Core docs: https://learn.microsoft.com/aspnet/core/
- Minimal APIs: https://learn.microsoft.com/aspnet/core/fundamentals/minimal-apis
- EF Core docs & performance: https://learn.microsoft.com/ef/core/ and https://learn.microsoft.com/ef/core/performance/
- JWT in ASP.NET Core: https://learn.microsoft.com/aspnet/core/security/authentication/jwtbearer
- Swagger / Swashbuckle: https://github.com/domaindrivendev/Swashbuckle.AspNetCore
- xUnit: https://xunit.net/
- Integration testing: https://learn.microsoft.com/aspnet/core/test/integration-tests

---

## Interview Topics & Most-Asked Questions (Phase 4)

1. ASP.NET Core Middleware Pipeline
   - Q: Explain middleware ordering and how to write custom middleware.
   - Learn: https://learn.microsoft.com/aspnet/core/fundamentals/middleware

2. Dependency Injection & Service Lifetimes
   - Q: Scoped vs Transient vs Singleton — examples and pitfalls.
   - Learn: https://learn.microsoft.com/aspnet/core/fundamentals/dependency-injection

3. EF Core Queries, Tracking, and N+1 Problems
   - Q: How to detect and fix N+1 queries; when to use AsNoTracking?
   - Learn: https://learn.microsoft.com/ef/core/querying/tracking / https://learn.microsoft.com/ef/core/performance/

4. JWT & Refresh Token Flow
   - Q: How to implement refresh tokens securely and mitigate replay attacks.
   - Learn: https://auth0.com/learn/json-web-tokens/ / https://learn.microsoft.com/aspnet/core/security/authentication/jwtbearer

5. API Versioning & Documentation
   - Q: How to version APIs and generate accurate OpenAPI docs.
   - Learn: https://swagger.io/docs/ / https://github.com/domaindrivendev/Swashbuckle.AspNetCore

6. Integration Testing Strategies
   - Q: Integration test patterns: in-memory DB vs. test container vs. real DB.
   - Learn: https://learn.microsoft.com/aspnet/core/test/integration-tests

Practice approach:
- Implement and unit-test an authentication service and write an integration test that exercises login → token refresh flow.