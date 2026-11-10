# atlas-render-farm

Atlas render farm — **code and workflows only.**

## Hard rule: no media in this repo

No personal, client, or production media is ever committed here. Media flows
in and out at runtime via Hugging Face dataset links (inputs are downloaded
from a HF dataset repo at job start; outputs are uploaded back the same way).
This repo holds only workflow definitions and small helper scripts.

## Runner

- One self-hosted runner: Jasveer's MacBook Air (M4, macOS, arm64)
- Labels: `mac-m4`, `self-hosted`, `macOS`, `ARM64`
- The runner is registered to **this repository only** — never to any other
  repo, and never org-wide.

## Safety

- All workflows are **`workflow_dispatch` only** — nothing runs on push, pull
  request, or any external event, so no untrusted code can ever execute on
  the Mac.
- Jobs take explicit HF dataset paths as dispatch inputs; they do not accept
  code from outside this repo.

## Workflows

| Workflow | What it does |
|---|---|
| `ffmpeg-render.yml` | Download input media from a HF dataset, render/assemble with ffmpeg (styled `.ass` captions supported), upload the result back to a HF dataset |
| `upscale-mps.yml` | RealESRGAN upscale batch on the M4 GPU (MPS) — one job at a time (`concurrency: 1`) |
| `qc-transcribe.yml` | Whisper transcription + line-completeness QC report for a media file from a HF dataset |

## Secrets

Workflows expect one repository secret:

- `HF_TOKEN` — a Hugging Face **read/write** token for the storage fleet
  account that owns the input/output datasets. Set it in
  Settings → Secrets and variables → Actions. Never commit it.
