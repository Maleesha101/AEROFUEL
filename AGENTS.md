# AEROFUEL Agent Instructions

## Project purpose

AEROFUEL is an intentionally vulnerable cybersecurity laboratory simulating an airport fuel logistics platform.

The primary educational objective is to demonstrate a realistic SSRF-led attack chain across microservice trust boundaries.

## Core design principle

Do not turn this project into a generic vulnerable-web-app collection.

The intended chain is:

1. Public manifest-import functionality
2. SSRF
3. Internal manifest-worker access
4. Internal information disclosure
5. Service credential exposure
6. Operations API authentication
7. Broken authorization
8. Unauthorized fuel-order operation
9. Flag

Every vulnerability should have a clear role in this chain.

## Safety boundary

This repository is a controlled training environment. Vulnerabilities must remain isolated to the lab and must not depend on attacking third-party infrastructure.

All required attack-chain components must run locally.

Do not introduce real credentials, production secrets, or external service dependencies.

## Implementation principles

- Prefer realistic production-style naming.
- Avoid obvious names such as `/ssrf`, `/exploit`, or `/get-secret`.
- Keep internal services on private Docker networks.
- Expose only the public gateway to the host.
- Keep the final flag inaccessible until the intended chain is completed.
- Make the challenge deterministic and resettable.
- Do not accidentally introduce unrelated vulnerabilities.
- Document intentional vulnerabilities for instructors, not in learner-facing pages.

## Intended vulnerability scope

Primary vulnerabilities:

- SSRF
- Internal information disclosure
- Service credential exposure
- Broken authorization / excessive service privilege

Avoid adding unrelated SQL injection, XSS, RCE, command injection, path traversal, or weak-password vulnerabilities unless a future design explicitly requires them.

## Documentation

Keep these documents synchronized with implementation:

- `docs/architecture.md`
- `docs/threat-model.md`
- `docs/attack-surface.md`
- `docs/vulnerability-model.md`
- `docs/development.md`
- `docs/defender-guide.md`

Detailed instructor exploitation material must not be placed in the public learner README.

## Testing

Every intentional vulnerability must have a security regression test.

The final project must support:

```bash
docker compose up --build
```

and a complete reset through:

```bash
./scripts/reset.sh
```

## Development workflow

Implement the platform in small, reviewable stages:

1. Documentation and architecture
2. Docker/network foundation
3. Gateway
4. Manifest worker
5. Operations API
6. Database/audit services
7. Frontend
8. Intentional vulnerabilities
9. End-to-end attack-chain tests
10. Defender/instructor material

Do not mark a phase complete until its tests and documentation are consistent.
