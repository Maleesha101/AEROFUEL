# AEROFUEL Threat Model

## Threat actor

The primary threat actor is an unauthenticated external user who can interact with the public AEROFUEL application.

The exercise assumes the attacker has no direct access to the Docker internal network.

## Assets

- Fuel manifests
- Fuel orders
- Internal service credentials
- Internal API functionality
- Operational data
- Audit records
- Integrity of fuel-release workflows

## Trust boundaries

```
External User
    |
    v
Public Gateway
    |
    +----> Internal Manifest Worker
    |
    +----> Internal Operations API
    |
    +----> Audit Service
             |
             +----> Database / Cache
```

The most important boundary is between the public gateway and private internal services.

## Threats

### T1 — Server-Side Request Forgery

A user-controlled manifest URL can cause the gateway to initiate a server-side request.

### T2 — Internal service exposure

The SSRF primitive may allow access to services that are not directly reachable from the Internet.

### T3 — Internal information disclosure

Operational endpoints may reveal service topology and configuration.

### T4 — Credential exposure

A diagnostic/configuration endpoint may expose a service credential.

### T5 — Excessive service privilege

A service credential intended for one business function may be accepted for higher-impact operations.

### T6 — Business workflow manipulation

An attacker may cross multiple trust boundaries and perform an operation that should require stronger authorization.

## Security goals

- External users must not reach internal services directly.
- URL retrieval must be constrained to explicitly approved destinations.
- Internal diagnostics must not expose secrets.
- Service credentials must follow least privilege.
- Authorization must be checked per operation.
- Sensitive business operations must generate auditable events.

## Out of scope

The lab is not intended to teach:

- SQL injection
- XSS
- RCE
- command injection
- password cracking
- exploitation of third-party systems
