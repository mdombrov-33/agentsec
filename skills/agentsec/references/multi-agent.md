# Multi-agent systems

Read when an orchestrator delegates work, workers share state, or one agent consumes another's messages. A single application calling several models does not automatically need a distributed identity system; inspect the actual process and trust boundaries.

## AGENT-01: Preserve scope through delegation

Draw authority separately from message flow: initiating subject → task → parent → worker → target resource. Record allowed resources/actions, purpose, audience, expiry and parent lineage at each edge. Effective authority must stay within the originating grant and the worker's assigned task; a child cannot expand scope because it has a powerful workload credential.

Example: a parent may read tenant A invoices and ask a worker to compare totals. A worker request to send the invoices to an external address is outside that assignment even if a mail connector exists. Inspect the executor's enforcement rather than relying on the parent's natural-language role instruction.

Use trusted task records or scoped capabilities supported by the architecture. Keep model-proposed policy fields separate from authenticated attributes. Apply [identity/tools](identity-and-tools.md) at the final resource, including retries and callbacks. Test a valid child operation and a wider action/resource, expiry and parent revocation.

## AGENT-02: Validate message origin, audience and freshness

Inspect transport authentication, sender/recipient binding, task identity, schema/version, replay handling and shared storage. Cryptographic integrity can prove a sender relationship without proving a claim is true or the requested action is authorized. Signed malicious content is still malicious content.

For separate trust domains, use authenticated transport and message integrity/freshness appropriate to the threat. For in-process workers, trusted invocation and isolated task records may suffice. Do not require signatures on every internal function call.

Test a message for another task/worker, a duplicate or stale message, forged supervisor authority and an incompatible schema. Validate at ingestion and before effects. Keep untrusted tool results or peer observations from being promoted to policy because they arrived from a recognized agent.

## AGENT-03: Prevent shared-state laundering

Trace original source → worker claim → shared memory → parent conclusion. Several agents repeating one source do not provide independent corroboration. Retain source lineage, uncertainty and trust class through handoffs. A summary dropping the original source's restrictions can create a wider read audience.

Apply [memory](memory-and-compaction.md) to writes, reads and reduced state. Verify one poisoned shared entry across a later task, cross-tenant/shared-user identities and revoked source access. Observe context and any downstream effect. Preserve a valid handoff and legitimate shared result.

## AGENT-04: Bound propagation and stop the task tree

Find who permits spawning, maximum depth/fan-out, queued work, shared budget and revocation propagation. Per-worker limits do not cap a recursive tree. [Runtime](runtime-and-recovery.md) owns reservation, retries, cancellation and recovery; follow the same task accounting across all workers.

Probe a child spawning beyond its assignment, duplicate delivery and a stale worker returning after parent stop. Check whether an accepted late result can reintroduce quarantined data or dispatch a new action. Record orphaned work and unavoidable in-flight effects.

**Sources:** [OWASP Agentic AI — Threats and Mitigations, v1.1](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/) and [Securing Agentic Applications, v1.0](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/) provide agent architecture, communication and cascading-risk background. Authority tracing, replay probes and test sequences here are project guidance. These checks do not claim to verify every multi-agent architecture or Byzantine-consensus property.
