# Phase 8 — Portfolio & Production Readiness

## Objective
Polish portfolio projects, harden security, prepare for interviews, and prepare production-grade documentation for deployable projects.

## Outcomes
- Production-ready portfolio with 3–4 fully deployed apps.
- Architecture docs, runbooks, and onboarding README.
- Interview prep and hiring materials.

## Checklist & Deliverables
- Portfolio site linking to deployed projects with case studies.
- At least two full-stack apps deployed to production-ready environments.
- Architecture diagrams, cost estimates, scaling plan for main app.
- Security review: OWASP Top 10 checklist, dependency scanning, secret rotation policy.
- Interview kit: coding challenges, system design notes, behavioral stories.

## Final Projects & Case Studies
- Social Media MVP (core features documented and deployed).
- E-commerce App: payment flow (sandbox) and admin panel.
- Project Management Tool: real-time features and deployment pipeline.

## Acceptance Checklist
- [ ] Portfolio website has case studies and links to source code.
- [ ] CI/CD pipelines with production and rollback steps are documented.
- [ ] Security checklist completed for deployed apps.

## Resources (Focused)
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- System Design Primer: https://github.com/donnemartin/system-design-primer
- SRE guidance: https://sre.google/sre-book/
- Diagramming: https://excalidraw.com/ or https://app.diagrams.net/

---

## Interview Topics & Most-Asked Questions (Phase 8)

1. System Design: Scalable Architectures
   - Q: Design an e-commerce checkout system: components, data model, scaling plan.
   - Learn: System Design Primer: https://github.com/donnemartin/system-design-primer

2. Caching & CDN Strategies
   - Q: How to design cache layers (browser, CDN, edge, origin) and handle invalidation?
   - Learn: Cloudflare caching best practices: https://developers.cloudflare.com/cache/

3. OWASP Top 10 & Secure Architecture
   - Q: Give examples of OWASP Top 10 vulnerabilities and mitigation strategies.
   - Learn: https://owasp.org/www-project-top-ten/

4. SLIs/SLOs/Runbooks & Incident Postmortems
   - Q: How to set SLOs and write a postmortem after an outage?
   - Learn: Google SRE book, SLO chapter: https://sre.google/sre-book/chapters/service-level-objectives/

5. Cost Optimization & Production Hardening
   - Q: How to analyze cloud costs and propose optimization strategies for production systems?
   - Learn: Azure cost management docs and cloud provider cost optimization guides.

Practice approach:
- Prepare 2–3 system design case studies (draw diagrams, list trade-offs, show scaling and cost considerations). Prepare postmortem sample for a hypothetical incident.