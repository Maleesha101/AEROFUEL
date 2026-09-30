# AEROFUEL Defender Guide

This document describes the defensive design that should be applied when hardening the intentionally vulnerable laboratory.

## SSRF

Prefer an explicit destination allowlist over a blacklist.

Controls should include:

- allowlist approved domains where possible
- allow only required URL schemes
- parse and normalize URLs using a trusted library
- validate resolved destinations
- block private, loopback, link-local, and other unintended address ranges
- validate every redirect destination
- restrict outbound network access with egress controls
- avoid unnecessary server-side URL fetching

## Internal services

Internal network placement is not a replacement for authentication.

Internal services should:

- authenticate requests
- authorize operations independently
- avoid trusting source network location as proof of identity
- expose only required endpoints
- disable debug/configuration endpoints in production

## Credential handling

Do not expose:

- bearer tokens
- API keys
- passwords
- connection strings
- runtime secrets

through diagnostics or configuration endpoints.

Use a dedicated secret-management mechanism where appropriate.

Rotate credentials after suspected exposure.

## Authorization

Separate:

```
Authentication = Who are you?
Authorization  = What are you allowed to do?
```

Every sensitive operation should check explicit permissions.

A manifest-import service should not automatically be allowed to approve or release fuel orders.

## Network segmentation

Use network controls so that a public application component cannot unnecessarily reach:

- databases
- management interfaces
- internal administration APIs
- metadata services
- unrelated infrastructure

## Detection

Monitor:

- unusual outbound requests
- requests to private address ranges
- access to internal diagnostic paths
- unusual service-to-service operations
- service identities performing unexpected operations
- anomalous fuel-order releases

Audit records should capture the actor, operation, resource, and timestamp.
