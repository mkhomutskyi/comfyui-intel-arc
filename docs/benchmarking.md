# Benchmark existing models

Read the dated report's protocol and match its actual local filenames, loader
nodes, resolution, sampler and steps. This script deliberately downloads nothing.
Run from the Docker host with permission to read the relevant /proc and sysfs
counters; incomplete telemetry is reported, not silently treated as a GPU peak.
It submits image jobs and refuses an occupied ComfyUI queue. Stop other workloads
on the same card yourself; it does not stop or unload another application.

```bash
python scripts/benchmark_comfyui.py --base-url http://127.0.0.1:8188 \
  --container comfyui-intel-arc --pci-address YOUR_DOMAIN_BUS_DEVICE_FUNCTION \
  --output evidence/new-run
python scripts/summarize_comfyui.py evidence/new-run
```

Replace the PCI placeholder with the actual BDF reported by `lspci -D`, not a
copied example address. Use a new output directory each run. The bundled
workflows under docs/benchmarks are ComfyUI API graphs, not GUI editor saves.
Use them with /prompt or the documented benchmark client. They require existing
licensed model files and matching nodes. There is no generic quality ranking.
