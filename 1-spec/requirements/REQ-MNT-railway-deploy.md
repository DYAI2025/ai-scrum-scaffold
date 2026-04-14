# REQ-MNT-railway-deploy: Deployable on Railway

**Type**: Maintainability

**Status**: Approved

**Priority**: Must-have

**Source**: [CON-railway-deployment](../constraints/CON-railway-deployment.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

The application must build and deploy successfully on Railway using either Nixpacks auto-detection or a Dockerfile. Environment configuration must be managed via Railway environment variables.

## Acceptance Criteria

- Given the repository is connected to Railway, when a push to main occurs, then Railway builds and deploys the application without manual intervention
- Given the application requires configuration (Stripe keys, FuFirE API URL, etc.), when deployed, then all secrets are injected via Railway environment variables — not hardcoded or committed
- Given the deployment completes, when Railway performs a health check, then the application responds with HTTP 200 within 30 seconds of container start
- Given a deployment fails, when the build log is inspected, then the error is attributable to application code — not missing Railway-specific configuration
