# Evaluate security behavior

## Specify the experiment

Determine whether the user wants a plan, fixtures or execution. For execution, establish the authorized target, test accounts/data, allowed effects, request/spend limits, stop conditions and evidence handling. Reuse authorization already provided. Prepare local simulations for effects outside that permission.

Define each case as an attacker capability, delivery path, invariant, observable failure and legitimate task. Distinguish model manipulation from application harm: a model may follow an instruction while a tool gateway correctly blocks its effect. Conversely, a polite answer may accompany a leaked model request, external image fetch or poisoned memory write.

## Cover the actual boundaries

Choose the test layer and its oracle explicitly:

| Layer | Example observation | Claim it supports |
|---|---|---|
| Model behavior | Repeated live-model trials select an injected instruction | Observed influence under the recorded configuration |
| Implementation | Recording model receives another tenant's marker | Exposure at that application boundary |
| System | Controlled file/network probe is denied by the configured executor | Tested isolation behavior within that environment |
| Runtime/task | Cancelled task cannot add a ledger entry; ordinary task still commits | Tested stop/effect semantics on the exercised path |

One layer's pass does not establish the others. A scripted model returning chosen text is useful at the implementation layer but cannot supply a live-model attack success rate.

Select cases from the relevant domains and accessible architecture. Include direct and indirect content, tool observations and persistent state where present. Add conventional parser/renderer, identity and execution checks alongside semantic attacks.

Record attacker access separately for each case. Direct user input, control of a retrieved document, a permitted memory write, a compromised tool and a stolen credential are different starting positions. Test which independent boundaries remain at each applicable position rather than describing them all as prompt injection. A privileged assumed-compromise probe establishes containment at that privilege level; it does not show that an unauthenticated attacker can obtain it. Increasing simulated leverage does not authorize attacks on additional live systems.

Use benign controls, secure variants and representative languages/modalities. Vary attack delivery or semantics rather than only one blocked string. Include delayed activation and repeat tasks when testing persistence. Keep model-based judges isolated from evaluated content and calibrate them against deterministic evidence and human labels.

For containment testing, inject an assumed-malicious proposal directly into the application boundary. Label this as an assumed-compromise test; it does not measure the probability a live model produces the proposal. Use repeated model trials separately when that probability matters.

## Execute and retain evidence

Reset fixture state between trials. Record system, model, prompt/policy, tools, data snapshot, available decoding settings, trial counts, observed boundary events and outcomes. Use synthetic canaries and controlled sinks; minimize sensitive trace data. Stop when an authorized limit is reached or an unexpected external effect occurs, preserving the evidence needed to investigate.

Compute results using explicit denominators and consequence-specific success criteria. Report benign task success and false positives alongside attack success. Small samples and passing scanners support bounded conclusions; avoid converting them into claims of universal security.

Finish with results actually observed, unexecuted cases, limitations and retest priorities. Confirmed failures should yield a minimal regression, a nearby variant, a benign control and a check at the owning deterministic boundary. A plan is complete when someone can execute it without inventing its scope, effect oracle or stop conditions.

**Source basis:** [OWASP GenAI Red Teaming Guide, v1.0](https://genai.owasp.org/resource/genai-red-teaming-guide/) distinguishes model, implementation, infrastructure and runtime assessment. The effect-level oracles, access-position comparisons and completion criteria here are project guidance.
