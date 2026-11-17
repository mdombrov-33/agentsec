# Model and training lifecycle

Read when the project selects or hosts models, fine-tunes weights/adapters, promotes feedback into training, or changes inference routes. For a hosted model with no training access, inspect the provider/data boundary and deployment configuration; do not assume an application attacker can alter its weights.

## MODEL-01: Identify what can change learned behavior

Separate inference-time context, retrieval sources, persistent memory, training examples, feedback labels, weights and adapters. An injected document can influence today's answer without modifying the model; storing it for later training creates another path with different writers and recovery requirements.

Trace who can submit, label, curate and promote each dataset. Follow conversation logs, thumbs-up signals, issue reports and model-generated answers into fine-tuning jobs. User feedback establishes what a user submitted; it does not establish that an answer is correct, authorized or safe to train on.

Separate raw submissions, reviewed data and promoted training snapshots. Preserve writer identity, acquisition time, transformations, sensitive-data classification and source/deletion lineage. Inspect whether an attacker can dominate the update through volume, duplicates or control of labels. A content scan cannot replace authorization to change a consequential training corpus.

## MODEL-02: Review the release as a deployed configuration

Identify the model checkpoint/provider version, tokenizer, adapter, prompt, tool set, policy, guardrail configuration, retrieval snapshot and relevant inference settings that actually run. A base-model result does not establish the behavior of another adapter, prompt or tool configuration. Hosted aliases and fallback routes can change that configuration without a repository diff.

Compare the candidate against the current deployment on the affected tasks, known failures, representative benign inputs and suspected trigger conditions. Aggregate quality can hide narrow backdoor behavior. Keep promotion authority outside the model and training job. Use staged rollout and a recoverable known-good configuration according to the deployment's scale.

[Supply chain](supply-chain.md) owns artifact acquisition, executable loaders and integrity. This reference owns data-to-model promotion and the relationship between the assessed configuration and the one serving requests. Disabling a route should prevent a fallback from silently selecting the same quarantined model or adapter.

## MODEL-03: Identify the claimed privacy failure precisely

Distinguish:

| Claim | Evidence needed |
|---|---|
| Context disclosure | Restricted bytes enter or leave the application's request path |
| Training-data extraction | Output reproduces protected training material with a credible training origin and exposure path |
| Membership inference | A defined inference method distinguishes known members/nonmembers under stated access and error rates |
| Inversion or property inference | A reconstruction or inferred attribute is compared with ground truth and an appropriate baseline |
| Model extraction | Access permits obtaining artifacts or reproducing functionality beyond the allowed use |

A canary supplied in the current prompt establishes no claim about memorization. A guessed personal detail does not establish training membership. Identify query access, attacker knowledge, budget and affected population before selecting a method. Use synthetic authorized data where possible; bound query campaigns and record false positives as well as successful disclosures.

Apply controls to the actual path: training-data minimization, dataset/artifact access, provider retention settings, response exposure and cumulative query budgets. Encryption at rest does not stop disclosure through an authorized inference endpoint. Statistical privacy guarantees require specialist analysis of the training procedure and parameters; do not infer them from a product label.

## MODEL-04: Remove learned contamination at its source

Follow a poisoned training source into dataset versions, jobs, checkpoints, adapters and deployed routes. Quarantining a source or clearing conversation memory does not remove influence already learned into weights. Determine whether restoration requires excluding affected data and retraining from a known-good checkpoint, removing an adapter, or retiring a model version.

Coordinate route/credential containment and downstream artifact recall through [runtime and recovery](runtime-and-recovery.md). Recheck the triggering behavior and legitimate tasks before reintroducing capability. State limits when the provider's training lineage or deletion behavior cannot be inspected.

**Sources:** [OWASP LLM04:2025](https://genai.owasp.org/llmrisk/llm042025-data-and-model-poisoning/) covers training/fine-tuning poisoning and backdoors; [OWASP LLM02:2025](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/) covers sensitive-data exposure. The CLLMSE Official Study Handbook, v1.0, provides training background on distinct privacy attacks and lifecycle controls. The inspection and evidence requirements above are project guidance, not a complete ML-robustness or privacy assessment.
