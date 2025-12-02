# Phase 6 — Cloud & DevOps (Weeks 23–28)

## Objective
Containerize apps, build CI/CD pipelines, deploy to Azure, and add monitoring/observability. Gain practical DevOps proficiency.

## Outcomes
- Dockerized frontend and backend with docker-compose.
- CI/CD pipelines using GitHub Actions or Azure DevOps.
- Deploy apps to Azure App Service, Azure SQL, Blob Storage, and optionally AKS.
- Implement logging, metrics, and basic alerts.

## Week-by-week Plan
Week 23 — Dockerization
- Create Dockerfiles for frontend (node) & backend (.NET).
- Multi-stage builds and minimizing image sizes.
- Docker Compose for local orchestration.

Week 24 — CI with GitHub Actions
- Setup workflows to build/test/publish artifacts.
- Implement PR checks, linters, and test steps.

Week 25 — Azure Deployment
- Deploy App Service, configure connection strings, and secrets.
- Set up Azure SQL and Blob Storage.

Week 26 — Secrets & Environment Management
- Use Azure Key Vault and GitHub Secrets.
- Use managed identities for Azure resources.

Week 27 — Monitoring & Logging
- Add Application Insights (backend) and front-end monitoring.
- Centralized logs with Serilog sinks.

Week 28 — Orchestration (Optional)
- Intro to AKS and Helm charts for microservices.
- CI/CD for Kubernetes (Flux/ArgoCD overview).

## Projects
- Dockerized Full Stack App with CI/CD that deploys to staging.
- Monitoring dashboard project demonstrating alerts and metrics.

## Acceptance Checklist
- [ ] Docker images built and pushed to registry.
- [ ] CI pipeline passes on PRs.
- [ ] Application deployed to Azure with working telemetry.

## Resources (Focused)
- Docker docs: https://docs.docker.com/get-started/
- Dockerfile best practices: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- GitHub Actions: https://docs.github.com/actions
- Azure for developers: https://learn.microsoft.com/azure/developer/
- Azure Key Vault: https://learn.microsoft.com/azure/key-vault/
- Application Insights: https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview
- Terraform: https://www.terraform.io/docs/ and Azure Bicep: https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview

---

## Interview Topics & Most-Asked Questions (Phase 6)

1. Docker & Multi-stage Builds
   - Q: How do multi-stage builds reduce image size? Show a simple multi-stage Dockerfile.
   - Learn: https://docs.docker.com/develop/develop-images/multistage-build/

2. CI/CD Pipelines (GitHub Actions)
   - Q: Explain a GitHub Actions pipeline that builds, tests, and deploys an app.
   - Learn: https://docs.github.com/actions/learn-github-actions/introduction-to-github-actions

3. Azure Deployment Patterns & Secrets
   - Q: How to deploy a .NET API and React frontend to Azure securely (secrets, Key Vault, Managed Identities)?
   - Learn: App Service quickstart: https://learn.microsoft.com/azure/app-service/quickstart-dotnetcore  
     Key Vault: https://learn.microsoft.com/azure/key-vault/overview

4. Infrastructure-as-Code (Terraform/Bicep)
   - Q: What is IaC and how to manage environment drift?
   - Learn: Terraform: https://www.terraform.io/docs/ ; Bicep: https://learn.microsoft.com/azure/azure-resource-manager/bicep/overview

5. Observability & Tracing
   - Q: How to instrument an app for logs, metrics, and traces; sample with App Insights/OpenTelemetry.
   - Learn: https://opentelemetry.io/docs/ and https://learn.microsoft.com/azure/azure-monitor/

Practice approach:
- Create a pipeline that builds artifacts and pushes Docker images; be ready to explain the YAML and secret handling.