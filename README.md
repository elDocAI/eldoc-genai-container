# elDoc GenAI Deployment Guide

## Container Deployment via Podman / Docker Compose

## 1. Introduction

This document describes how to deploy **elDoc with GenAI capabilities** using **Podman Compose** or **Docker Compose**.

It covers:

* Compose-based orchestration
* AI model configuration
* Embedded Qdrant vector database
* Environment configuration via `.env`

For general elDoc container information (TLS, base configuration, system requirements, reverse proxy, licensing, etc.), refer to: [https://docs.eldoc.online/latest/installation-guide/deployment-container](https://docs.eldoc.online/latest/installation-guide/deployment-container)

---

## 2. Deployment Structure

The GenAI deployment consists of:

* `compose.yaml` – Container orchestration definition
* `.env` – Environment configuration (primary configuration file)

Both files must be placed in the same directory.

### Services Created

The Compose deployment starts two containers:

| Service  | Purpose                                              |
| -------- | ---------------------------------------------------- |
| `eldoc`  | elDoc AIO container with GenAI features enabled      |
| `qdrant` | Internal vector database used for embeddings and RAG |

Only HTTPS (port 443) from the elDoc container is exposed to the host.

---

## 3. Prerequisites

Before starting, ensure:

* Podman 4+ with `podman compose`, or Docker with `docker compose`
* Valid elDoc license including the AI module
  (Community License includes AI features by default)
* Proper AI provider credentials (OpenAI, Bedrock, or ONNX models)

For base container requirements, refer to the official container deployment guide.

---

## 4. Configuration

All GenAI-related configuration is done in the `.env` file.

It is **not recommended** to modify AI parameters directly in `compose.yaml`, unless you fully understand the configuration structure.

---

## 5. Host Configuration

Set the public hostname used to access elDoc in the `.env` file:

```
ELDOC_HOST=eldoc.domain.com
```

The hostname must:

* Be resolvable via DNS
* Match the TLS certificate hostname (if using a valid certificate)

TLS configuration details are covered in the main container documentation.

---

## 6. Enabling GenAI

GenAI functionality is enabled via:

```
ELDOC_AI=true
```

This parameter is already set in `compose.yaml` and should not be modified.

---

## 7. AI Model Configuration

The deployment supports configuration for:

* Chat model
* Agent model
* Vision-Language (VL) model
* Embedding model
* Reranking model

All configuration is controlled via environment variables in `.env`.

---

## 8. Vector Database (Qdrant)

Qdrant runs as an internal service within the Compose deployment.

* Not exposed externally
* Persistent storage via the `qdrant_data` volume
* Connected via internal container network

No additional configuration is required for the default setup.

---

## 9. Starting the Deployment

From the directory containing both files:

### Detached mode

```
podman compose up -d
```

or

```
docker compose up -d
```

### Foreground mode (for debugging)

```
podman compose up
```

---

## 10. Stopping the Deployment

```
podman compose down
```

or

```
docker compose down
```

Volumes are preserved unless explicitly removed.

---

## 11. Logs and Monitoring

Check running containers:

```
podman ps
```

View elDoc logs:

```
podman logs eldoc-aio
```

View Qdrant logs:

```
podman logs eldoc-qdrant
```

---

## 12. Security Notes

* Do not commit `.env` to version control.
* Protect API keys and Bedrock credentials.
* Restrict host firewall access to port 443 only.
* For production-grade security configuration, refer to the main container deployment guide.

---

## 13. Additional Documentation

For the additional documentation refer to: [https://docs.eldoc.online/latest/installation-guide/deployment-container](https://docs.eldoc.online/latest/installation-guide/deployment-container)

