# AEROFUEL Development Guide

## Current phase

Repository foundation and security-lab specification.

## Planned implementation order

### Phase 1 — Foundation

- Docker Compose structure
- network definitions
- documentation
- environment conventions
- service skeletons

### Phase 2 — Core platform

- gateway
- manifest worker
- operations API
- database
- audit service

### Phase 3 — Business workflow

- suppliers
- manifests
- fuel orders
- approval/release workflow

### Phase 4 — Security-lab behavior

- SSRF scenario
- internal diagnostics
- credential exposure
- authorization weakness
- flag condition

### Phase 5 — Validation

- unit tests
- integration tests
- security regression tests
- end-to-end attack-chain verification

### Phase 6 — Training material

- instructor guide
- defender guide
- hints
- reset/observation tooling

## Development requirements

Every service should have:

- a clear responsibility
- health checks
- environment configuration
- tests
- service-specific documentation where needed

## Configuration

Never commit real secrets.

Use:

```
.env.example
```

for documented development variables.

Challenge secrets should be generated locally and deterministically where required by the exercise.

## Docker

The completed lab must be runnable using:

```bash
docker compose up --build
```

Use explicit service networks and avoid unnecessary host port exposure.

## Testing

Before merging implementation work:

```bash
docker compose config
```

should succeed.

Application and security tests must pass.

## Reset

The final project should provide:

```bash
./scripts/reset.sh
```

which restores the challenge to its initial state.
