# Phase 7 — Advanced Concepts (Ongoing)

## Objective
Explore advanced architecture & patterns: microservices, GraphQL, gRPC, SSR/SSG, performance, and security hardening.

## Outcomes
- Understand microservice trade-offs and communication patterns.
- Implement GraphQL APIs and/or gRPC microservices where appropriate.
- Use SSR/SSG patterns (Next.js) for SEO-sensitive pages.
- Security best practices and performance tuning at scale.

## Focus Areas
- Microservices: domain boundaries, event-driven architecture, message brokers (Azure Service Bus / RabbitMQ).
- GraphQL: schema design, pagination, caching, persisted queries.
- gRPC: high-performance RPC for inter-service comms.
- SSR/SSG: Next.js or prerendering strategies.
- Observability: distributed tracing (OpenTelemetry), structured logs.

## Suggested Projects
- Microservices prototype with a message bus and eventual consistency.
- GraphQL gateway combining multiple services.
- SSR product catalog using Next.js with API fallback.

## Acceptance Checklist
- [ ] One prototype using microservice communication.
- [ ] GraphQL endpoint with pagination and caching.
- [ ] Observability implemented end-to-end for one service.

## Resources (Focused)
- Microservices guidance: https://learn.microsoft.com/dotnet/architecture/microservices/
- Event-driven patterns: https://microservices.io/patterns/index.html
- Apollo GraphQL: https://www.apollographql.com/docs/
- Hot Chocolate (GraphQL .NET): https://chillicream.com/docs/hotchocolate
- gRPC: https://grpc.io/docs/ and https://learn.microsoft.com/aspnet/core/grpc/
- OpenTelemetry: https://opentelemetry.io/docs/

---

## Interview Topics & Most-Asked Questions (Phase 7)

1. Microservices vs Monoliths
   - Q: Compare monolith vs microservices. When are microservices appropriate?
   - Learn: https://martinfowler.com/articles/microservices.html / https://microservices.io/

2. Event-driven Architecture, Idempotency & Ordering
   - Q: How to implement eventual consistency and guarantee idempotency in event processing?
   - Learn: https://learn.microsoft.com/dotnet/architecture/microservices/#event-driven-architecture

3. GraphQL Design & N+1 Problem
   - Q: How to avoid N+1 in GraphQL and secure endpoints?
   - Learn: Apollo best practices: https://www.apollographql.com/docs/ ; Hot Chocolate: https://chillicream.com/docs/hotchocolate

4. gRPC Use Cases vs REST
   - Q: When to use gRPC versus REST? Show proto example and streaming scenario.
   - Learn: https://grpc.io/docs/ / https://learn.microsoft.com/aspnet/core/grpc/

5. Distributed Tracing & Observability
   - Q: How to trace a request across multiple services using OpenTelemetry?
   - Learn: https://opentelemetry.io/docs/

Practice approach:
- Build a mini system with two services communicating via events; document how you ensure ordering and idempotency and demonstrate with logs/traces.