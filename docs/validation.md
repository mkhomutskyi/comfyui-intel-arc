# Validation record

October18,2026: the pinned Intel image passed Intel's acceptance script in a
non-privileged, network-disabled B50 container. A1024FP8 workflow generated an
image in19.83seconds in one initial-load observation. The benchmark report
separately records warmed three-run medians and its exact quality limitations.

October19 repository split: preserved the source/image pins, model filenames,
workflows and recorded measurements. The benchmark client now takes its
container name and PCI BDF explicitly and no longer calls an LLM service's unload
API. The new arguments, Python/JSON/YAML, local documentation links and
publication scans passed; the code migration did not run a new full image
benchmark. The Compose example remains localhost-bound and requires user
device/group/model paths. No models were downloaded during this split.

Static CI is not GPU inference validation. Changing Torch, Kitchen, AIMDO,
native kernels or ComfyUI requires a separately built candidate and repetition
of the image acceptance and saved workflows. Other Arc architectures, broader
model coverage and GUI/browser acceptance are not implied by the B50 record.
