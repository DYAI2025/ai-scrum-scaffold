# CON-railway-deployment: Deployment on Railway

**Category**: Technical

**Status**: Active

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

The application must be deployable on Railway as the hosting platform.

## Rationale

Railway is the chosen infrastructure provider. All deployment, CI/CD, and environment configuration must be compatible with Railway's platform constraints (Dockerfiles or Nixpacks, environment variables, health checks).

## Impact

Build output must be a static site or a lightweight server container. Vite's static build output (`dist/`) is directly deployable. If FuFirE API integration requires a backend-for-frontend proxy, it must run as a Railway service.
