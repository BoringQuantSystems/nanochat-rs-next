# Language Model Experiment

`nanochat-rs-next` is a separate Rust experiment for training and evaluating tiny language models against nanochat baselines. It shares the organization namespace but is not part of the `nanotrade` critical path.

## Language

**Language Model Experiment**:
The Rust-first context for reproducible tiny language-model training, sampling, ablation, parity, and benchmarking.
_Avoid_: trading module

**Corpus**:
The input text used to construct tokenizer vocabulary, token pairs, and train/validation splits.
_Avoid_: dataset unless comparing to upstream

**Tokenizer**:
The character-level mapping between corpus text and token IDs.
_Avoid_: encoder when discussing the project surface

**Training Run**:
A configured execution of model training over a corpus with explicit engine, optimizer, schedule, seed, and artifact settings.
_Avoid_: job when discussing experiment semantics

**Model Kind**:
The architecture family selected for a run, currently bigram or mini-gpt.
_Avoid_: mode

**Engine Mode**:
The runtime implementation selected for a run, currently scalar or tensor.
_Avoid_: model kind

**Optimizer**:
The training update rule selected for a run, currently SGD or AdamW where supported.
_Avoid_: schedule

**Learning-Rate Schedule**:
The step-wise multiplier applied to the base learning rate.
_Avoid_: optimizer

**Checkpoint**:
The persisted model/run state written during training and before evaluation.
_Avoid_: artifact when the persisted state specifically resumes or audits training

**Sampler**:
The mode that generates text from a trained or checkpointed model.
_Avoid_: generator

**Ablation**:
A controlled comparison that changes one or more model or training settings to explain behavior.
_Avoid_: benchmark

**Baseline**:
The external or reference implementation used for comparison, currently centered on upstream `nanochat`.
_Avoid_: competitor

**Parity Run**:
A run designed to compare local behavior against upstream or Python reference behavior.
_Avoid_: benchmark when correctness comparison is primary

**Artifact**:
A persisted output of a run, such as checkpoints, generated text, metrics, logs, or summaries.
_Avoid_: file

## Relationships

- **Corpus → Tokenizer**: One corpus defines one tokenizer vocabulary for a run.
- **Training Run → Model Kind**: One training run chooses one model kind.
- **Training Run → Engine Mode**: One training run chooses one engine mode.
- **Training Run → Optimizer**: One training run chooses one optimizer.
- **Training Run → Checkpoint**: One training run can create many checkpoints.
- **Training Run → Artifact**: One training run produces many artifacts.
- **Ablation → Training Run**: One ablation contains multiple comparable training runs.
- **Baseline → Training Run**: A baseline run provides a comparison point for one or more local runs.
- **Parity Run → Baseline**: One parity run compares local behavior against one baseline or reference.
- **Language Model Experiment ↛ Trading Orchestration**: This context does not provide runtime contracts to `nanotrade`.

## Example Dialogue

Dev: Should `nanochat-rs-next` appear as a dependency in `nanotrade`?

Domain expert: No. It is a separate experiment, not a trading-stack support module.

Dev: A benchmark produces checkpoints and metrics. What should we call them together?

Domain expert: Artifacts. That keeps the language independent of the specific file format.

Dev: Is tensor mode the same thing as mini-gpt?

Domain expert: No. Tensor is an engine mode; mini-gpt is a model kind.

## Flagged Ambiguities

**nano prefix**:
The `nanochat-rs-next` name shares the `nano` prefix, but it is not part of the nano trading stack.

**mode vs model kind**:
Use "engine mode" for scalar/tensor and "model kind" for bigram/mini-gpt.

**benchmark vs parity run**:
Use "benchmark" for speed or quality measurement and "parity run" for behavioral equivalence checks.
