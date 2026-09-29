# AGENTS.md — layer-python-ml

Standalone candy repo for the `python-ml` layer — a CUDA-ready Python 3.13 ML
environment (PyTorch, transformers, vLLM, llama.cpp). The candy lives in
`charly.yml` at the repo root: the `require:` on `layer-cuda`, the nested `candy:`
composition of `llama-cpp`, the `env:` block, the vLLM pip step, the `check:`
assertions, and the embedded `skill:` entity projected into the marketplace
corpus as `/charly-languages:python-ml`.

Canonical files:

- `charly.yml` — the `python-ml:` candy entity and the `python-ml-skill:` skill
  entity.
- `pixi.toml` / `pixi.lock` — the core ML Python environment.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-languages:python-ml` — the owning skill. The Tier 2 environment-owner
  meta-layer, the pixi.toml it owns, the `llama-cpp` sub-candy composition, and
  the vLLM wheel install. Load before editing or troubleshooting the candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the nested `candy:` composition list,
  service declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- `charly box validate` at the repo root checks the manifest parses and
  validates.
- The candy's `plan:` `check:` steps are the functional evidence: the pixi
  interpreter exists and PyTorch, transformers, and vLLM each import and print a
  version.
- Regenerate `pixi.lock` whenever `pixi.toml` changes — the build installs with
  `pixi install --frozen` and fails loudly on a stale lock.

## Modify this repo

- Edit the `python-ml:` candy entity AND the `python-ml-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a version
  or path change not mirrored in the skill leaves the corpus stale.
- Keep the build order (pixi environment → llama-cpp → vLLM wheel) intact; the
  vLLM wheel is pip-installed after the pixi environment exists.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
