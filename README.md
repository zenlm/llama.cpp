# llama.cpp

Optimized llama.cpp for Zen model inference. Supports GGUF quantization for all Zen models.

[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## Overview

High-performance C/C++ inference engine optimized for the Zen model family. Includes custom quantization profiles, KV cache optimizations, and architecture-specific kernels for Zen4 models.

## Features

- Full GGUF support for all Zen models (Zen4, Zen4 Ultra, Zen4 Coder Pro)
- Optimized MoE routing for Zen4 Coder Pro inference
- Metal (Apple Silicon), CUDA, and Vulkan acceleration
- 128K context with efficient KV cache management
- Speculative decoding support
- OpenAI-compatible API server

## Build

```bash
git clone https://github.com/zenlm/llama.cpp
cd llama.cpp

# macOS (Metal)
cmake -B build -DLLAMA_METAL=ON
cmake --build build --config Release -j

# Linux (CUDA)
cmake -B build -DLLAMA_CUDA=ON
cmake --build build --config Release -j

# CPU only
cmake -B build
cmake --build build --config Release -j
```

## Inference

```bash
# Interactive chat
./build/bin/llama-cli \
  -m zen4-ultra-Q4_K_M.gguf \
  -c 8192 \
  -n 512 \
  --chat-template chatml \
  -i

# API server
./build/bin/llama-server \
  -m zen4-coder-pro-Q4_K_M.gguf \
  -c 32768 \
  --host 0.0.0.0 \
  --port 8080

# Batch processing
./build/bin/llama-cli \
  -m zen4-Q4_K_M.gguf \
  -p "Explain quicksort in Python:" \
  -n 1024
```

## Quantization

Convert Zen models to GGUF:

```bash
# Convert from HuggingFace
python convert_hf_to_gguf.py zenlm/zen4-ultra --outfile zen4-ultra-F16.gguf

# Quantize
./build/bin/llama-quantize zen4-ultra-F16.gguf zen4-ultra-Q4_K_M.gguf Q4_K_M
```

## Supported Models

| Model | Recommended Quant | RAM (Q4_K_M) |
|-------|------------------|--------------|
| Zen4 Ultra (405B) | Q4_K_M | ~240 GB |
| Zen4 Coder Pro (80B MoE) | Q4_K_M | ~48 GB |
| Zen4 Coder (32B) | Q4_K_M | ~20 GB |
| Zen4 (32B) | Q4_K_M | ~20 GB |
| Zen4 Mini (8B) | Q8_0 | ~8 GB |

## Related

- [zen4-ultra](https://github.com/zenlm/zen4-ultra) — Frontier-scale model
- [zen4-coder-pro](https://github.com/zenlm/zen4-coder-pro) — Professional code generation
- [Zen LM](https://github.com/zenlm) — Full model family

MIT · [Zen LM](https://zenlm.org) · [Hanzo AI](https://hanzo.ai)
