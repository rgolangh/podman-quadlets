# Podman Quadlets

A curated collection of ready-to-use [Podman Quadlet](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html) definitions for self-hosted services and local AI inference.

## Quick Start

Install any quadlet with a single command:

```console
podman quadlet install <directory> --reload-systemd --replace
```

Or install directly from a URL:

```console
podman quadlet install https://github.com/rgolangh/podman-quadlets/blob/main/redis/redis.container --replace
```

---

## AI / LLM Inference

Local model serving on Intel Arc GPUs via [llama.cpp](https://github.com/ggml-org/llama.cpp). All llama.cpp quadlets auto-download models from HuggingFace on first start, pass through `/dev/dri` for GPU access, and expose an OpenAI-compatible API.

| Quadlet | Port | Description |
|---------|------|-------------|
| **[gemma-3-1b-arc](gemma-3-1b-arc/)** | 8086 | Gemma 3 1B IT (Q4_K_M) on the Vulkan backend. A lightweight model with an 8K context window, ideal for quick local tasks. |
| **[gemma-4-e2b-arc](gemma-4-e2b-arc/)** | 8085 | Gemma 4 E2B IT (Q4_0) on the Vulkan backend. A capable 2B-class model with a 16K context window. |
| **[gemma-4-e4b-arc](gemma-4-e4b-arc/)** | 8084 | Gemma 4 E4B IT (Q4_K_M) on the Vulkan backend. The largest local Gemma variant with a 16K context window and KV cache quantization to reduce VRAM usage. |
| **[gemma-speculative-arc](gemma-speculative-arc/)** | 8087 | Gemma 4 E2B with a Gemma 2 2B draft model for speculative decoding. The small draft model proposes tokens that the larger model verifies, improving generation throughput. |
| **[gemma-inference](gemma-inference/)** | 8088 | Gemma 4 E4B via Google's LiteRT-LM runtime instead of llama.cpp. Includes a custom-built container image with an OpenAI-compatible Node.js API server. |
| **[mistral7b-arc](mistral7b-arc/)** | 8083 | Mistral 7B Instruct v0.3 (Q4_K_M) on the SYCL/oneAPI backend. A 7B parameter model with a 4K context window for general-purpose instruction following. |
| **[phi35-arc](phi35-arc/)** | 8081 | Phi 3.5 Mini Instruct (Q4_K_M) on the SYCL/oneAPI backend. Microsoft's compact 3.8B model, well-suited for reasoning and code generation tasks. |

### Pulling the Server Images

```console
# Vulkan backend (recommended for Intel Arc)
podman pull ghcr.io/ggml-org/llama.cpp:server-vulkan

# SYCL/oneAPI backend (used by Mistral and Phi)
podman pull ghcr.io/ggml-org/llama.cpp:server-intel
```

---

## Self-Hosted Services

| Quadlet | Port | Description |
|---------|------|-------------|
| **[wordpress](wordpress/)** | 8080 | Full WordPress stack with a MariaDB database, linked via a dedicated Podman network. Just install and start blogging. |
| **[nextcloud](nextcloud/)** | 11001 | Nextcloud All-in-One master container for rootless Podman. Manages its own sub-containers for a complete self-hosted cloud with files, calendar, and more. |
| **[pihole](pihole/)** | 8053 | Pi-hole DNS sinkhole for network-wide ad blocking. Binds DNS to a dedicated interface IP to avoid conflicts with the host's resolver. |
| **[maybe](maybe/)** | 3000 | Maybe personal finance app with a PostgreSQL backend. A self-hosted, open-source alternative for tracking your finances. |

---

## Databases & Messaging

| Quadlet | Port | Description |
|---------|------|-------------|
| **[postgresql](postgresql/)** | 5432 | PostgreSQL 16 with persistent storage. Includes an optional init container for first-run database setup. |
| **[redis](redis/)** | 6379 | Lightweight Redis instance for caching and pub/sub. Runs as a non-root user for improved security. |
| **[valkey](valkey/)** | 6379 | Valkey 8, the community-driven Redis fork, with persistent volume storage. A drop-in Redis replacement with an open-source license. |
| **[redpanda](redpanda/)** | 19092 | Redpanda single-node cluster with the Redpanda Console. A Kafka-compatible streaming platform that runs without ZooKeeper or a JVM. |

---

## Templates

| Quadlet | Description |
|---------|-------------|
| **[local-build](local-build/)** | Skeleton quadlet demonstrating how to use a `.build` unit to compile a local Containerfile into an image and run it. Useful as a starting point for custom services. |
