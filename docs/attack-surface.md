# AEROFUEL Attack Surface

## Public attack surface

The main planned public functionality is the supplier manifest workflow.

### Manifest import

```http
POST /api/v1/manifests/import
Content-Type: application/json

{
  "manifest_url": "https://supplier.example/manifest.json"
}
```

The server retrieves the supplied resource.

This endpoint is the primary SSRF attack surface.

## Internal attack surface

The following services are intended to remain private:

| Service | Example Port | Exposure |
|---|---:|---|
| Manifest Worker | 8081 | Internal only |
| Operations API | 8082 | Internal only |
| Audit Service | 8083 | Internal only |
| PostgreSQL | 5432 | Internal only |
| Redis | 6379 | Internal only |

The exact implementation may change as development proceeds.

## Investigation questions

A learner should be able to investigate:

1. Does the server retrieve the supplied URL?
2. Can the server reach destinations unavailable to the client?
3. What internal services can be reached?
4. What information is returned by those services?
5. Are credentials or service identities exposed?
6. What permissions does a discovered service credential actually have?
7. Can a low-privilege service perform a sensitive business operation?

## Non-goals

Do not expose every internal service on localhost merely to make testing easier. The network boundary is part of the learning objective.
