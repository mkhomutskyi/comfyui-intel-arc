# ComfyUI on Intel Arc GPUs

A pinned Docker setup guide, portable Compose example and reproducible image
benchmarks. **Validated hardware: Intel Arc Pro B50, 16GB.** Other Arc cards are
not claimed tested; the native BMG image target is not an Alchemist recipe.

| Start here | Purpose |
|---|---|
| [Setup](docs/setup.md) | Build the tested Intel source, select the GPU, start/validate and roll back |
| [Measurements](docs/benchmarks/2026-10-18-comfyui-b50.md) | Models, seeds, timing, VRAM and quality limitations |
| [Compose example](examples/comfyui-b50/compose.yaml) | Localhost UI, separate writable data, read-only model store |
| [Benchmark instructions](docs/benchmarking.md) | Run against explicitly named existing weights |

## Quick start

Install Docker with Compose and a Linux kernel/Intel compute runtime supporting
your card. Confirm the actual card/render nodes and their group IDs first.
Build `llm-scaler-omni:0.2.0-b2-comfyui-bmg` from the exact Intel source revision
in the setup guide; this is a local build, not a published registry image.

```bash
git clone https://github.com/mkhomutskyi/comfyui-intel-arc.git
cd comfyui-intel-arc/examples/comfyui-b50
cp .env.example .env
# Set COMFY_DATA and actual device group IDs; place licensed models there.
mkdir -p /srv/comfyui/{models,input,output,user}
docker compose config --quiet
docker compose up -d
curl -fsS http://127.0.0.1:8188/system_stats
```

Open http://localhost:8188. Follow [setup](docs/setup.md) for required model
directory layout and GPU acceptance checks before submitting a workflow.
No model weights are included or automatically downloaded. The example does
not require privileged mode, unconfined AppArmor or an overlaid host checkout.

## Version and validation boundary

Recorded October18,2026: ComfyUI0.39.0, Torch2.14.0+xpu, Intel Omni0.2.0-b2,
Kitchen0.2.37, AIMDO0.5.5. The pinned runtime and saved image workflows passed
on B50, including a non-privileged network-disabled container. The October19
repository split and portable benchmark argument changes passed static checks;
they were not a new GPU benchmark. See the [validation record](docs/validation.md). Do not treat newer package availability as
validated compatibility with this image's native binary ABI.

Z-Image FP8 and SDXL Lightning worked with the recorded dynamic-memory policy.
One BF16 recipe executed but produced poor images; a separate longer-schedule
diagnostic improved quality. INT8 failed at the loader, not from proven OOM.
See the dated report before quoting any timing or16GB fit claim.

## Project and licensing

Repository recipes/scripts are Apache-2.0; ComfyUI, Intel integration code,
custom nodes and model weights keep their own licenses. Nothing is relicensed
by these instructions. [Notices](NOTICE), [security](SECURITY.md).

Separate projects: [Illama2](https://github.com/mkhomutskyi/illama2) for LLMs,
[Speech on Arc](https://github.com/mkhomutskyi/speech-intel-arc) for TTS/STT.

[OpenWebUI integration guide](https://github.com/mkhomutskyi/openwebui-local-ai-stack)
connects chat, images and speech with persistent data and shared family access.

