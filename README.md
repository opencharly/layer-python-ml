# python-ml

A CUDA-ready Python 3.13 ML environment for OpenCharly images — PyTorch,
transformers, and a vLLM inference runtime.

The `python-ml` candy owns the `pixi.toml` that builds the core ML Python
environment under `~/.pixi/envs/default`: PyTorch (cu130),
torchvision/torchaudio, transformers, accelerate, and the full vLLM
runtime-dependency closure. After the pixi environment is built, a user-phase
task pip-installs the vLLM nightly wheel (`--no-deps`) into that same
interpreter.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `python-ml` (Tier 2 environment-owner meta-layer) |
| Interpreter | `~/.pixi/envs/default/bin/python` |
| Libraries | PyTorch, transformers, vLLM (nightly wheel) |
| Requires | [`layer-cuda`](https://github.com/opencharly/layer-cuda) |
| Composes | [`layer-llama-cpp`](https://github.com/opencharly/layer-llama-cpp) via `candy:` |
| Install files | `charly.yml`, `pixi.toml`, `pixi.lock` |
| Service / port | none — no Jupyter server |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list (the shipped box
uses the `nvidia` base):

```yaml
my-ml-box:
  candy:
    base: nvidia
    candy:
      - '@github.com/opencharly/layer-python-ml:v2026.243.0515'
```

Then, inside the built image (or on a dev host):

```bash
~/.pixi/envs/default/bin/python -c "import torch; print(torch.__version__)"
~/.pixi/envs/default/bin/python -c "import vllm; print(vllm.__version__)"
~/.pixi/envs/default/bin/python -c "import transformers; print(transformers.__version__)"
```

The candy's `plan:` asserts the interpreter exists and that PyTorch,
transformers, and vLLM all import and print a version.

## Layout

- `charly.yml` — the `python-ml:` candy entity (the `require:` on `layer-cuda`,
  the `candy:` composition of `llama-cpp`, the `env:` block, the vLLM pip step,
  and the `check:` assertions) and the embedded `python-ml-skill:` skill entity.
- `pixi.toml` / `pixi.lock` — the core ML Python environment.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-languages:python-ml`
- GPU base: `/charly-distros:cuda`, `/charly-distros:nvidia`
- Sub-candy: `/charly-jupyter:llama-cpp`
- Sibling boxes: `/charly-jupyter:jupyter-ml`, `/charly-jupyter:unsloth-studio`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
