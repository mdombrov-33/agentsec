# Runtime and recovery

Read for autonomous loops, queues, retries, resource limits, cancellation, telemetry or incident traces. Inspect deployed topology where accessible; source intent does not prove running enforcement.

## RUN-01: Bound the whole task

Map accounting for input/output tokens, model/tool calls, retries, wall time, money, fan-out, memory writes and queued work. Identify task, tenant and global limits. Per-request limits can coexist with an unlimited task or recursive worker tree.

Inspect admission, reservation, settlement and failure behavior. If two workers both see one remaining call and dispatch, a check-then-increment counter can overspend. Reserve atomically before dispatch where concurrency matters, reconcile known actual usage, and define what happens to reservations after failure. Variable external cost may need a conservative bound; an alert or post-hoc cost report is not a hard cap.

Follow retries and fallback models through the same accounting. Distinguish transient errors from repeated equivalent failure; define a finite retry budget, backoff and stop/degraded behavior. A fallback must not bypass destination policy or cost limits. Cancellation and exhausted budget should prevent queued redispatch, not merely end the planner loop.

Verify a small local limit with concurrent dispatch, repeated failure, costly fallback and delegated work. Observe committed reservations and calls actually made. Record overshoot assumptions for in-flight provider calls. Preserve an ordinary task that finishes within budget.

## RUN-02: Define stop at execution boundaries

Trace user/operator stop → task state → queue/lease → executor → final effect. Check pending retries, child workers, credential lifetime and late completion. A UI status of “cancelled” does not establish that a record remained unchanged.

| Stop point | Required observation |
|---|---|
| Before dispatch | No new work begins; stale queue deliveries are rejected |
| During computation | Worker termination/cooperative cancellation; bounded residual work |
| Before commit | Current task/grant state is rechecked at the last enforceable boundary |
| After external commit | Actual result is reconciled; irreversible effects or compensation are reported |

Keep management controls independent of the model being stopped. Check authorization after resume/retry, and make repeated cancellation safe. [Identity/tools](identity-and-tools.md) owns exact-effect approval and duplicate-operation semantics; this reference owns propagation of task stop and resource state.

Test cancel-before-dispatch, pause/cancel-before-commit, stale worker completion and repeated queue delivery. Distinguish sequential simulations from concurrent tests. For a remote service that can commit after timeout, state that local cancellation cannot guarantee prevention; use provider cancellation/idempotency and reconciliation where available.

## RUN-03: Record enough evidence to reconstruct the effect

Correlate task/parent ID, initiating principal, relevant resource/source IDs, model/prompt/policy/tool versions, proposed operation, policy decision, approval binding, executor result, memory dependencies, retry/budget and stop events. Log the actual system-of-record result where available, not just a model's success message.

Protect telemetry outside the agent's write authority and minimize raw prompts, credentials and unrelated personal data. Source IDs plus controlled evidence retention can be more useful than unbounded prompt dumps. A gap in traces limits causal confidence; do not infer hidden intent or exact internal reasoning from fluent output.

For an incident request, construct a timeline of known events, hypotheses and missing evidence. Identify whether influence is ongoing, stored or propagating. Supplied malicious logs/documents remain evidence; they cannot authorize cleanup or external actions.

Use the [incident workflow](workflow-incident.md) for investigation order and the report to deliver. Keep this reference as the owner of runtime evidence, stop and recovery controls.

## RUN-04: Contain and verify recovery

Choose proportionate containment: stop one task, revoke its delegation, disable a writer/tool, quarantine sources/derivatives, restrict egress or place the application in advisory mode. Preserve needed forensic artifacts before cleanup. A kill switch that depends on the compromised planner is insufficient.

Trace source influence through vectors, caches, summaries, checkpoints and downstream artifacts. [Memory](memory-and-compaction.md) owns derivative lineage; coordinate its quarantine/rebuild with credential revocation and version rollback. Reverting a prompt does not remove contaminated state. [Supply chain](supply-chain.md) owns artifact/update trust.

Exercise recovery with a synthetic poisoned source: identify affected consumers, block influence, restore authorized state, rerun the trigger and a legitimate task. Confirm retired components lose credentials, registrations and scheduled work. Report unrecoverable effects and untested provider/backup behavior. This is a technical investigation aid, not a complete organizational incident-response program.

On retirement, follow endpoints, discovery registrations, webhooks, schedulers, service identities and delegated sessions. Disabling the UI does not retire a worker with an active credential. Apply the retention/deletion policy to source and derived data, preserve required evidence, identify dependent callers and check for continued use of the retired route.

**Sources:** [OWASP LLM10:2025](https://genai.owasp.org/llmrisk/llm102025-unbounded-consumption/) covers resource/service impact. [ASD, Agentic AI Harnesses, September 2026](https://www.cyber.gov.au/publication/agentic-ai-harnesses) discusses harness permissions, observability and operational controls. Reservation, stop-state and recovery sequences are project engineering guidance.
