# Development plan

## Goal

Let a user upload a short music loop and generate a set of instrumental variations that retain useful musical material while exploring different arrangements and sounds. Italo-disco is the target style. Vocals are out of scope.

Text prompts are not part of the first prototype. First establish whether conditioning on the uploaded audio can produce variations that sound useful; add prompt controls after that is demonstrated.

## MVP scope

- Accept a short audio loop in common formats.
- Validate and prepare it for the selected model without changing its musical content unnecessarily.
- Generate several variations from the same source, with repeatable seeds where supported.
- Preview and export the generated audio as WAV.
- Keep each result associated with its source and generation settings.

**Not in the MVP:** vocals, lyric generation, user-authored prompts, model training, hosted accounts, advanced mastering, or claims of commercial clearance.

## Proposed libraries

| Need | Proposed library/tool | Role |
|---|---|---|
| Model inference | **PyTorch** + **Hugging Face Transformers** | Load and run a candidate audio-conditioned music model. Keep this behind a model adapter so another model can be evaluated without rewriting the app. Install a PyTorch build that matches the actual CPU/GPU environment. |
| First model to benchmark | **MusicGen Melody** through Transformers | A candidate for melody/audio-guided variation. Treat it as an experiment, not a confirmed fit: assess whether it changes the arrangement in the desired way, not just whether it follows the source melody. |
| Audio arrays | **NumPy** | Represent, validate, and pass waveform data between audio I/O and inference. |
| WAV I/O | **SoundFile** (`soundfile`) | Read/write uncompressed WAV and inspect sample rates/channels. Use it for the canonical internal/export format. |
| Upload format decoding | **FFmpeg** executable | Decode common user uploads (such as MP3) and convert to the model's required input format. Install/test the executable separately; a Python wrapper does not replace it. |
| Optional loop analysis | **librosa** | Use only if benchmarks need BPM/beat-grid estimates, trimming, or audio features. It is not the music generator and should not be required for basic file conversion. |
| Tests | **pytest** | Test input validation, conversion, variation metadata, and export without loading a large model. |
| Later local UI | **Gradio** | Add a simple upload, progress, preview, and download interface after the generation path works. Not needed for the first library/model experiment. |

### Dependency strategy

- Start with a small, model-independent audio layer (`numpy`, `soundfile`) and an FFmpeg system dependency.
- Keep `torch`, `transformers`, and model-support packages in the optional `musicgen` extra. Install the PyTorch build that matches the actual CPU/GPU environment using its official selector.
- Do not add `audiocraft`, `torchaudio`, `diffusers`, or a web framework by default. Add one only if the selected model or implemented feature requires it.
- The project Python requirement is 3.11 or newer. `requirements.txt` installs the project, MusicGen inference extra, and test tools; the PyTorch install must still match the user's hardware.
- Keep model weights and generated audio out of Git; store results in an ignored output directory.

## Model fit and quality constraints

MusicGen Melody is a reasonable first benchmark because its model card demonstrates generation guided by an audio melody. It is not yet a decision: melody guidance is not necessarily the same as turning an arbitrary uploaded loop into a convincing, stylistically consistent arrangement.

The Transformers MusicGen documentation describes audio-prompted generation with a 30-second total context limit, where the provided audio prompt counts toward that limit. This makes short loops a more suitable first test and may constrain longer arrangements. The documented MusicGen checkpoints use a 32 kHz audio codec, so exporting at a higher sample rate or bit depth does not restore detail that was not generated.

**License constraint:** the MusicGen Melody model card lists the checkpoint weights as CC-BY-NC 4.0. This is a non-commercial license; this checkpoint is not suitable for a commercial product unless the rights situation changes and is reviewed. The software libraries' licenses do not override the model weights' license.

Before adopting any model, verify its current license and intended-use terms, hardware/memory requirements, supported Python and PyTorch versions, audio-conditioning behavior, and whether its output quality is adequate for the project's distribution goals.

## Milestones

### 1. Audio-conditioned model benchmark

- Test a small set of short, rights-cleared loop examples with MusicGen Melody.
- Generate multiple seeds per input using a fixed internal conditioning setup; user-authored prompts remain deferred.
- Compare against at least one alternative audio-to-audio or melody-conditioned model if the first results do not meet the variation goal.
- Evaluate source relationship, musical coherence, artifacts, output duration, generation speed, memory use, and license.

**Exit criteria:** choose a model only if listening tests show consistently useful loop-conditioned variations on the available hardware.

### 2. Audio ingestion and generation core

- Implement upload validation and a single canonical audio representation.
- Decode supported formats with FFmpeg; preserve the original upload and make any resampling/channel conversion explicit.
- Define a model adapter taking prepared source audio and variation settings, returning audio plus metadata.
- Export playable WAV variations and report conversion/model failures explicitly.

**Exit criteria:** one source loop reliably produces a named set of valid, playable variations.

### 3. Basic user workflow

- Add preview, progress reporting, and download of individual variations or a set.
- Save source filename, model identifier, seed, input conversion details, and generation settings with each result.
- Add a simple local UI only after the core is stable; Gradio is the proposed lightweight option.

**Exit criteria:** a user can upload, generate, compare, and retrieve results without command-line audio handling.

### 4. Prompts and studio workflow

- Add user-authored prompts as an optional control and measure whether they improve results without overriding the uploaded loop.
- Add production exports and optional stems only if the chosen model supports meaningful stem separation or stem generation.
- Add loudness/peak checks and higher-quality formats based on measured model output, not upsampling alone.

## First implementation task

Build a small benchmark harness for the input-loop workflow with `soundfile`, NumPy, FFmpeg, pytest, and a candidate PyTorch/Transformers MusicGen Melody adapter. Keep generation dependencies optional until the model and hardware compatibility are confirmed.

## References

- [Transformers MusicGen documentation](https://huggingface.co/docs/transformers/main/en/model_doc/musicgen)
- [MusicGen Melody model card](https://huggingface.co/facebook/musicgen-melody)
- [SoundFile documentation](https://python-soundfile.readthedocs.io/)
- [librosa documentation](https://librosa.org/doc/latest/index.html)
- [PyTorch local installation selector](https://pytorch.org/get-started/locally/)
