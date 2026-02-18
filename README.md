# elDoc GenAI Deployment Guide

## Container Deployment via Podman / Docker Compose

---

## 🚀 Quick Start (3 Steps)

If you just want to get elDoc GenAI running:

### 1️⃣ Download deployment files

```bash
git clone https://github.com/elDocAI/eldoc-genai-container.git
cd eldoc-genai-container/compose
```

### 2️⃣ Configure environment

Edit the `.env` file and set:

* `ELDOC_HOST`
* AI provider credentials
* Required model configuration

### 3️⃣ Start containers

```bash
podman compose up -d
```

or

```bash
docker compose up -d
```

elDoc will be available at:

```
https://<ELDOC_HOST>
```

For detailed configuration and production guidance, continue reading below.

---

## 1. Introduction

This document describes how to deploy **elDoc with GenAI capabilities** using **Podman Compose** or **Docker Compose**.

It covers:

* Compose-based orchestration
* AI model configuration
* Embedded Qdrant vector database
* Environment configuration via `.env`

For general elDoc container information (TLS configuration, base setup, system requirements, reverse proxy, licensing, etc.), refer to:
[https://docs.eldoc.online/latest/installation-guide/deployment-container](https://docs.eldoc.online/latest/installation-guide/deployment-container)

---

## 2. Download Required Files

Clone the repository:

```bash
git clone https://github.com/elDocAI/eldoc-genai-container.git
cd eldoc-genai-container/compose
```

Or download the ZIP archive:

```bash
curl -L -o eldoc-genai-container.zip https://github.com/elDocAI/eldoc-genai-container/archive/refs/heads/main.zip
unzip eldoc-genai-container.zip
cd eldoc-genai-container-main/compose
```

The `compose` directory contains:

* `compose.yaml`
* `.env`
* Deployment documentation

---

## 3. Deployment Structure

The GenAI deployment consists of:

* `compose.yaml` – Container orchestration definition
* `.env` – Environment configuration (primary configuration file)

Both files must be located in the same directory.

### Services Created

The Compose deployment starts two containers:

| Service  | Purpose                                              |
| -------- | ---------------------------------------------------- |
| `eldoc`  | elDoc AIO container with GenAI features enabled      |
| `qdrant` | Internal vector database used for embeddings and RAG |

Only HTTPS (port 443) from the elDoc container is exposed to the host.

---

## 4. Prerequisites

Before starting, ensure:

* Podman 4+ with `podman compose`, or Docker with `docker compose`
* A valid elDoc license including the AI module (Community License includes AI features by default)
* Valid AI provider credentials (OpenAI, Bedrock, or ONNX models)

For base container requirements, refer to the official container deployment guide.

---

## 5. Configuration

All GenAI-related configuration must be performed in the `.env` file.

It is **not recommended** to modify AI parameters directly in `compose.yaml`, unless you fully understand the configuration structure.

---

## 6. Host Configuration

Set the public hostname used to access elDoc in the `.env` file:

```bash
ELDOC_HOST=eldoc.domain.com
```

The hostname must:

* Be resolvable via DNS
* Match the TLS certificate hostname (if using a valid certificate)

TLS configuration details are covered in the main container documentation.

---

## 7. Enabling GenAI

GenAI functionality is enabled via:

```bash
ELDOC_AI=true
```

This parameter is already defined in `compose.yaml` and should not be modified.

---

## 8. AI Model Configuration

The deployment supports configuration for:

* Chat model
* Agent model
* Vision-Language (VL) model
* Embedding model
* Reranking model

All model configuration is controlled via environment variables in `.env`.

Refer to the `.env` file for parameter descriptions and examples.

---

## 9. Vector Database (Qdrant)

Qdrant runs as an internal service within the Compose deployment.

* Not exposed externally
* Persistent storage via the `qdrant_data` volume
* Connected via the internal container network

No additional configuration is required for the default setup.

---

## 10. Starting the Deployment

From the directory containing both files:

### Detached mode

```bash
podman compose up -d
```

or

```bash
docker compose up -d
```

### Foreground mode (for debugging)

```bash
podman compose up
```

---

## 11. Stopping the Deployment

```bash
podman compose down
```

or

```bash
docker compose down
```

Volumes are preserved unless explicitly removed.

---

## 12. Logs and Monitoring

Check running containers:

```bash
podman ps
```

View elDoc logs:

```bash
podman logs eldoc-aio
```

View Qdrant logs:

```bash
podman logs eldoc-qdrant
```

---

## 13. Security Notes

* Do not commit `.env` to version control.
* Protect API keys and Bedrock credentials.
* Restrict host firewall access to port 443 only.
* For production-grade security configuration, refer to the main container deployment guide.

---

## 14. Additional Documentation

For further details, refer to: [https://docs.eldoc.online/latest/installation-guide/deployment-container](https://docs.eldoc.online/latest/installation-guide/deployment-container)