# Development plan

## Goal

Build a small, reliable tool that creates original Italo-disco-inspired instrumental music with AI. The first release will not generate vocals, lyrics, or singing.

## MVP boundaries

**In scope**

- Text prompt describing the desired instrumental track.
- Useful controls for generation, starting with duration and random seed; add tempo and other controls when supported by the selected model.
- Save generated music as WAV, with clear progress and useful error messages.
- A documented local workflow that does not require users to publish their generated files.

**Out of scope for the first release**

- Vocals, lyrics, speech, or voice cloning.
- Training a foundation model from scratch.
- A hosted service, user accounts, or a complex graphical interface.
- Claims of commercial clearance until the model and its license have been verified.

## Proposed milestones

### 1. Choose the generation approach

- Compare suitable text-to-music models for instrumental quality, prompt adherence, local inference support, hardware requirements, and license terms.
- Test short prompts for Italo-disco-inspired arrangements and confirm a reliable way to request instrumental-only output.
- Decide whether inference is local-first or uses an external service; record privacy, cost, and hardware tradeoffs.
- Keep model-specific code behind a small generation interface so the rest of the app is not tied to one provider.

**Exit criteria:** a chosen model and runtime, a documented setup path, and a short sample that meets the instrumental requirement.

### 2. Establish a reproducible generation baseline

- Create a minimal command-line workflow from prompt to audio file.
- Support a fixed random seed where available and validate duration and output path inputs.
- Convert model output to a consistent WAV format and verify sample rate, channels, duration, and non-empty audio.
- Surface model download, device, and inference errors rather than silently returning placeholder audio.

**Exit criteria:** a documented command produces a playable WAV with reproducible settings where the model supports them.

### 3. Add Italo-disco controls and iteration

- Refine prompt templates for genre traits without relying on named-artist imitation.
- Add supported controls (for example tempo, mood, instrumentation, and duration) and preserve the original prompt in generation metadata.
- Provide a small set of prompt examples and a repeatable listening/evaluation checklist.
- Measure generation time and memory use on the intended development hardware.

**Exit criteria:** users can steer a generated instrumental meaningfully and compare results across seeds.

### 4. Improve usability and quality

- Add batch variations and simple preview/playback only if the basic CLI workflow is reliable.
- Consider audio post-processing such as loudness normalization only after testing that it does not damage generated output.
- Document model attribution, license constraints, output ownership caveats, and safe handling of downloaded model assets.
- Add automated tests for input validation, file writing, and model-independent audio handling.

**Exit criteria:** installation, generation, and troubleshooting are understandable to a new user.

## Technical shape (provisional)

- Python package with a small CLI as the first interface.
- A model adapter separates prompt/settings and inference from WAV export and application logic.
- Keep large model weights and generated audio out of version control; store generated files under an ignored `outputs/` directory by default.
- Delay GPU/framework dependencies until the model choice is made. Record tested Python, operating system, and device requirements at that point.

## Decisions to make before implementation

1. **Inference:** local model, hosted API, or support both?
2. **Hardware target:** CPU-only compatibility, a specific GPU class, or cloud inference?
3. **Output:** WAV only for MVP, or also compressed formats?
4. **Use and distribution:** personal experimentation, or a release intended for broader/commercial use? This affects model/license selection and documentation.
5. **Interface:** CLI-first (recommended for MVP) or a browser-based UI?

## First implementation task

Evaluate candidate models against the milestone 1 criteria, using short instrumental prompts and documenting quality, runtime, hardware, and license evidence before adding heavyweight dependencies.
