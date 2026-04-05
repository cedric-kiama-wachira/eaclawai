# SAD-005 - Distroless multi-stage Docker build pattern

**Date:** 2026-04-05
**Status:** Approved
**Author:** Cedric (Lead AI Enterprise Architect)

---

## Executive Summary

This document describes the hardened container build process that meets ARCH-CON-08 (non-root, no shell).

All 21 containers follow the same pattern: build stage compiles/installs, final stage is distroless, runs as non-root user.

## Related Architecture Decisions

- **ADR-005 - Docker Distroless over standard containers**

## Build Pattern

**Dockerfile Pattern:**
```dockerfile
# Stage 1: Build
FROM python:3.11-slim as builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

# Stage 2: Runtime (distroless)
FROM gcr.io/distroless/python3.11-nonroot:latest
COPY --from=builder /root/.local /home/nonroot/.local
COPY app.py .
USER nonroot:nonroot
ENV PATH=/home/nonroot/.local/bin:$PATH
ENTRYPOINT ["python", "app.py"]
```

**Key Security Properties:**
- No shell (can't run bash, nc, or any OS utilities)
- Non-root user (can't write to /usr, /etc, or system directories)
- Minimal base image (10MB vs 200MB for Alpine)
- All dependencies frozen (no package manager to install backdoors)

## Technology Stack

- **Docker Engine - Distroless containers**

## Compliance

**ARCH-CON-08 Checklist:**
- ✅ No shell (distroless has no /bin/bash, /bin/sh, /usr/bin/sh)
- ✅ Non-root user (runs as nonroot:nonroot, UID 65532)
- ✅ Immutable filesystem (can't add tools after container starts)
- ✅ All dependencies pinned (no silent upgrades)

## Success Criteria

undefined

## Deployment

**Build Process:**
1. Developer pushes code to GitHub
2. GitHub Actions builds distroless image
3. Security scan (Trivy) finds 0 critical vulnerabilities
4. Image pushed to Docker registry
5. Kubernetes/Docker Compose deployment pulls image

**Debugging:**
- No shell inside container (can't exec into it)
- Debug containers deployed alongside for troubleshooting
- Logs streamed to Langfuse/Prometheus for observability

---

**Document Owner:** Cedric (Lead AI Enterprise Architect)
**Last Updated:** 2026-04-05
**Review Cycle:** Quarterly (minimum)
