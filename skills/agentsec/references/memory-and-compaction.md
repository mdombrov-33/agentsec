# Memory and compaction

Read for stored conversations, summaries, checkpoints, profiles, cross-task memory or feedback reused as knowledge. Review both transitions: what gets written, and what a later task may consume.

## MEM-01: Keep authority separate from remembered content

Classify the actual stores before prescribing controls:

| State | Authority | Required inspection |
|---|---|---|
| Raw message or tool observation | Evidence of what was received | Origin, subject, tenant, access, retention |
| Generated summary or inferred fact | Derived, potentially incorrect | Inputs, transformation version, trust class, expiry |
| User preference | User-controlled within allowed fields | Writer identity, field policy, task applicability |
| Verified address, entitlement or policy | Authority from its owning system | Verification source, version, revocation |
| Checkpoint or handoff | Saved execution state | Task identity, delegated scope, freshness, resume policy |

Trace the storage write API. Check whether model-generated `verified`, `role`, `tenant`, `approved` or `source_ids` fields are accepted as authoritative. A preference such as “always use this bank account” must not modify a verified payment destination by being recalled in context.

Authorize writes and reads in the storage service or application policy. Bind subject, tenant, memory class and purpose outside model-generated text. Keep entitlements and action grants in the system that owns them. Labels such as “unverified summary” help interpretation; they do not enforce access or prevent manipulation.

Verify a normal preference update and an attempted permission/profile update through the same interface. On a later task, inspect context and protected effects. A stored attack string establishes persistence; it does not by itself establish execution or privilege escalation.

## MEM-02: Retain transitive dependencies during reduction

Trace source → extraction → summary → later summary → consumer. Inspect whether transformations retain tenant/task scope, source identity and version, expiry, denied actions and unresolved claims. Include repeated reduction and a handoff whose recipient cannot see the original inputs.

Worked sequence:

```text
1. User can read document A. A contributes to summary S1.
2. S1 and document B are supplied to generate S2.
3. The source service revokes the user's access to A.
4. A new query retrieves no documents. The assistant considers reusing S2.
```

If S2 records only B, a read check can pass while replaying content derived from A. Enforce authorization over the dependencies of every derivative actually supplied. For a small implementation, retaining the union of supplied source dependencies is conservative:

```python
dependencies = current_source_refs | supplied_prior_summary.dependencies
```

Attach metadata in trusted orchestration code. Asking the model which sources influenced its summary is not a reliable access-control mechanism. Dependencies can include conversation or tool data, not just retrieved documents. Record source versions where edits or deletion change reuse policy; an ID-only ACL check does not establish freshness or semantic validity.

Before replay, resolve dependencies against current policy. Missing lineage needs an explicit fallback: discard the derivative, regenerate from permitted sources, or independently review it as appropriate to the task. Removing a few forbidden words from a mixed summary cannot establish that all restricted influence was removed.

A dependency union can over-restrict an output that did not use every supplied input. Record that tradeoff. More selective reuse requires trustworthy finer-grained provenance, not the model's unsupported assertion. Verify authorized summaries still work; deleting every summary disables the feature.

## MEM-03: Preserve constraints across resume

Compare original and compacted state for allowed resources, recipients, denied operations, expiry, unresolved approvals and source trust. Enforce constraints from external task/policy state on resume and at execution, even when the summary is wrong. Loading a checkpoint proves serialization compatibility, not current authority.

Probe a summary that omits a denial or broadens recipients. Observe the resumed proposal and actual effect separately. Test changed permissions while paused, stale checkpoint replay and a new task reusing old state. Use [identity and tools](identity-and-tools.md) for execution authorization and [multi-agent systems](multi-agent.md) for delegated handoffs.

## MEM-04: Recover contaminated derivatives

Identify consumers from source/dependency IDs, memory versions and task traces. Quarantine affected entries, prevent reads, rebuild from permitted known-good inputs, and rerun the triggering task. Inspect vectors, caches, checkpoints and downstream artifacts as well as raw sources. Preserve scoped forensic evidence before destructive cleanup.

Verify cross-tenant identical user IDs, repeated summarization, revocation with empty retrieval, changed/deleted sources, missing lineage, delayed reuse and benign reuse. State which are exercised locally. Source revocation tests do not establish deletion from backups or provider infrastructure.

**Sources:** [OWASP Agentic Top 10, 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) identifies memory/context poisoning; [Securing Agentic Applications, v1.0](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/) covers state and lifecycle controls. Dependency closure, replay checks and the worked sequence are project guidance, not framework clauses. Application-wide recovery belongs to [runtime and recovery](runtime-and-recovery.md).
