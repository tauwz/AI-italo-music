# AI Italo Generator

A Python project for generating original, instrumental Italo-disco-inspired music with AI. The initial scope is music without vocals.

## Status

Planning and project setup.

## Getting started

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

The virtual environment is local and is excluded from Git. Runtime dependencies will be added after the model and inference approach are selected.

## Development plan

See [DEVELOPMENT_PLAN.md](DEVELOPMENT_PLAN.md) for the staged roadmap, initial product scope, and open decisions.

## Initial product direction

- Generate original instrumental tracks from text prompts.
- Target Italo-disco characteristics such as sequenced synthesizers, steady dance grooves, and melodic hooks.
- Offer practical controls such as duration, tempo, and random seed as the model permits.
- Export playable audio (initially WAV); vocals and lyric generation are explicitly out of scope.
- Prefer a model and workflow whose license permits the intended use; document model and output limitations.
