# AI Italo Music

A Python project that takes an uploaded music loop and generates multiple instrumental Italo-disco-inspired variations. Vocal generation is out of scope.

## Status

Planning and project setup. The first technical milestone is to benchmark audio-conditioned generation models against real input loops before selecting one.

## Getting started

Install FFmpeg using your operating system's package manager, then install PyTorch
using the selector at [pytorch.org](https://pytorch.org/get-started/locally/) to
match your CPU or GPU setup. With the virtual environment active:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

The virtual environment is local and is excluded from Git. The dependency list is
in [pyproject.toml](pyproject.toml); `requirements.txt` installs the project with
the MusicGen and development extras. FFmpeg is an external system dependency, not
a Python package.

**Model license:** the MusicGen Melody checkpoint is licensed CC-BY-NC 4.0
(non-commercial). Do not use it for commercial purposes. Review the model card and
license before downloading or using the weights.

## Development plan

See [DEVELOPMENT_PLAN.md](DEVELOPMENT_PLAN.md) for the staged roadmap, initial product scope, and open decisions.

## Initial workflow

1. Upload a short music loop.
2. Generate several instrumental variations conditioned on that loop.
3. Preview and export the results as WAV files.

User-authored text prompts are a later feature. The initial prototype will focus on audio conditioning, reliable loop ingestion, and comparing variations.
