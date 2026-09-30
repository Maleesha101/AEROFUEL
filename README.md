# AEROFUEL

**AEROFUEL** is an intentionally vulnerable, containerized security laboratory that simulates an airport fuel logistics platform.

The lab is designed to teach how a realistic **Server-Side Request Forgery (SSRF)** vulnerability can become the first step in a multi-stage attack chain crossing internal trust boundaries.

## Scenario

Airport fuel suppliers submit digital manifests to AEROFUEL. The public API provides a legitimate manifest-import workflow that retrieves a supplier-provided URL.

The simulated architecture contains internal services that are not directly exposed to the Internet. Security weaknesses in the manifest-import workflow allow a learner to investigate how server-side network access can expose internal application functionality.

### Learning focus

- SSRF discovery and validation
- Internal service discovery
- Trust-boundary analysis
- Internal information disclosure
- Service-to-service credentials
- Authentication vs. authorization
- Least privilege
- Defensive SSRF controls

The challenge is deliberately designed as a **chain**, rather than a single endpoint that directly returns a flag.

## Planned attack-chain concept

```
Public Manifest Import
        |
       SSRF
        |
Internal Manifest Worker
        |
Internal Information
        |
Service Credential
        |
Operations API
        |
Authorization Failure
        |
Unauthorized Business Operation
        |
Flag
```

Detailed exploitation instructions are intentionally excluded from this learner-facing README.

## Project status

This repository currently contains the project foundation and security-lab specification. Application services will be implemented incrementally.

## Planned architecture

- Public API / gateway
- Manifest worker
- Operations API
- Audit service
- PostgreSQL
- Redis
- Optional training frontend

Only the public gateway should be exposed to the host in the completed lab.

## Requirements

- Docker
- Docker Compose
- Git

## Intended local startup

Once the implementation phase is complete:

```bash
docker compose up --build
```

## Intended reset

```bash
./scripts/reset.sh
```

## Documentation

- [Architecture](docs/architecture.md)
- [Threat Model](docs/threat-model.md)
- [Attack Surface](docs/attack-surface.md)
- [Vulnerability Model](docs/vulnerability-model.md)
- [Development Guide](docs/development.md)
- [Defender Guide](docs/defender-guide.md)

Instructor-only exploitation material will be maintained separately from learner-facing documentation.

## Security notice

AEROFUEL is intentionally vulnerable software for **authorized security training and local laboratory use only**. Do not deploy it to production or expose the intentionally vulnerable services to untrusted networks.
