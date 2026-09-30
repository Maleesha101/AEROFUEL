# AEROFUEL Architecture

## 1. System overview

AEROFUEL models an airport fuel logistics platform used to receive supplier manifests and manage fuel orders.

The intended production-style architecture is:

```
                    Internet
                       |
                       v
              +------------------+
              |  AEROFUEL Gateway|
              |    Public API    |
              +--------+---------+
                       |
                 private network
                       |
          +------------+-------------+
          |            |             |
          v            v             v
   Manifest Worker  Operations    Audit Service
       :8081          API :8082       :8083
          |            |
          +-----+------+ 
                |
                v
          PostgreSQL / Redis
```

## 2. Trust boundaries

### Boundary A — Internet to Gateway

The gateway is the public attack surface.

### Boundary B — Gateway to internal services

Internal services must not be directly exposed to the host.

The SSRF scenario exists because the gateway can make server-side requests into this internal network.

### Boundary C — Service-to-service authentication

Internal APIs use service credentials.

The challenge intentionally demonstrates why authentication alone is insufficient without operation-level authorization.

### Boundary D — Services to data stores

PostgreSQL and Redis are infrastructure dependencies and must not be exposed directly to the learner.

## 3. Planned services

### Gateway

Responsibilities:

- public HTTP API
- user-facing manifest import
- authentication/session handling
- request validation
- communication with internal services

### Manifest Worker

Responsibilities:

- retrieve supplier manifests
- validate manifests
- transform manifest data
- communicate with the operations API

This service contains the intentionally exposed internal diagnostic/configuration behavior used by the attack chain.

### Operations API

Responsibilities:

- fuel orders
- approvals
- release workflow
- service authentication
- authorization

The lab intentionally introduces excessive service privilege in this boundary.

### Audit Service

Responsibilities:

- record operational security events
- provide defensive investigation data

### PostgreSQL

Persistent business data.

### Redis

Caching/background coordination where required.

## 4. Network requirements

The completed Compose environment should use explicit networks.

Recommended:

```text
public-network
internal-network
```

Only the gateway should publish a host port.

Internal services should communicate using Docker service names.

## 5. Design constraints

- No dependency on public cloud infrastructure.
- No real credentials.
- Deterministic local behavior.
- Repeatable reset.
- Minimal unrelated vulnerabilities.
