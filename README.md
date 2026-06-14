A repository holding commonly used podman quadlets

Use this with https://github.com/rgolangh/pq for super fast and easy setup:

``` console
pq install wordpress --repo https://github.com/rgolangh/podman-quadlets
```

## Gemma on Intel Arc GPU

Quadlets for running Google Gemma models locally on Intel Arc GPUs using
[llama.cpp](https://github.com/ggml-org/llama.cpp) with the Vulkan backend.

All quadlets share a common pattern:
- Models are auto-downloaded from HuggingFace on first start via `ExecStartPre`
- GPU access via `/dev/dri` passthrough
- Full GPU offload (`-ngl 99`)
- KV cache quantization (`--cache-type-k q8_0 --cache-type-v q8_0`) to reduce VRAM usage
- OpenAI-compatible API on each quadlet's published port

### gemma-3-1b-arc

Gemma 3 1B IT (Q4_K_M) on port **8086**. Lightweight model with 8K context window.

### gemma-4-e2b-arc

Gemma 4 E2B IT (Q4_0) on port **8085**. 16K context window.

### gemma-4-e4b-arc

Gemma 4 E4B IT (Q4_K_M) on port **8084**. 16K context window. Previously ran on the
SYCL backend (`server-intel`), switched to Vulkan since benchmarks show it is 53-67%
faster on Intel Arc.

### gemma-speculative-arc

Gemma 4 E2B (Q4_0) with a Gemma 2 2B (Q4_K_M) draft model on port **8087**.
Uses speculative decoding — the small draft model proposes tokens that the larger
model verifies, improving generation speed.

### Install

```console
podman pull ghcr.io/ggml-org/llama.cpp:server-vulkan
podman quadlet install gemma-4-e2b-arc/ --reload-systemd --replace
```

