# Human oversight

Read when generated analysis informs a consequential human decision, or a reviewer approves an agent's proposed action. Inspect the information available to the reviewer, not just the presence of an approval button. Execution binding belongs to [identity and tools](identity-and-tools.md).

## REVIEW-01: Build the review view from authoritative state

Follow source records → generated explanation → displayed evidence → human decision → committed operation. Determine which displayed fields come from the model and which are independently resolved. An assistant can describe a change as routine while omitting the actual recipient, altered account or expanded disclosure.

Build the action preview from the same canonical operation that the executor will validate. Resolve resource identity and old/new values from the owning system. Present generated reasoning separately from these fields. Make the underlying sources accessible under the reviewer's authorization; a citation string or a fluent explanation is not an evidence package.

Inspect truncated fields, collapsed changes, ambiguous account names, hidden recipients, unit/currency changes and screenshots that omit material details. Verify that rejecting the preview leaves the operation uncommitted and that approval binds to what was actually shown. Use the tool reference for freshness, replay and commit-time enforcement rather than treating a usable preview as authorization by itself.

## REVIEW-02: Treat persuasive content as part of the attack path

An internal record can contain attacker-controlled fields. Trace authorship at field level: an employee nickname, support-ticket note or customer message does not become an organizational instruction because a trusted CRM returned it. Generated urgency, administrator claims and confident recommendations can carry those instructions into a human decision without any tool-policy bypass.

Inspect whether model output can impersonate application controls through HTML, Markdown links, badges, embedded images or instructions to enter credentials. [Execution and output](execution-and-output.md) owns rendering safety. Here, establish whether the interface makes source content look like an authenticated instruction or a verified system result.

Check a source with a false authority claim and an ordinary source with legitimate urgency. Observe whether the reviewer can inspect origin, supporting passages and contradictory evidence. Do not suppress every urgent message or security discussion as an attack. A warning that users cannot distinguish from normal content supplies little protection.

## REVIEW-03: Make oversight possible under normal workload

Identify who may approve, what expertise and source access they need, and what happens when no qualified reviewer is available. Inspect bulk approvals, timeout defaults, repeated prompts and paths that bypass review under load. An approval queue that silently releases actions after timeout defeats the intended control.

Place review at meaningful changes in authority, recipient, resource, cost or reversibility. Reuse authorization already given for unchanged permitted work. Check whether routine prompts obscure the exceptional action. Measure the review path using realistic volume and accessibility needs; counting clicks does not establish informed review.

## REVIEW-04: Keep inference and decision authority distinct

For decision support, trace the actual outcome: which person or system acts on the generated recommendation, and what error or manipulation could change that outcome? Require relevant evidence, uncertainty and unresolved contradictions to survive summarization. Model-produced confidence scores need calibration before being used as decision thresholds.

Keep the final decision and rationale attributable to the responsible decision maker or policy service. Provide a path to correct source facts and propagate the correction to stored recommendations and downstream records. A corrected chat answer does not undo a decision already recorded elsewhere. Specific professional, fairness or legal requirements need separate evidence for the deployment.

**Sources:** [OWASP Agentic Top 10, 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) includes human-agent trust exploitation. [OWASP LLM09:2025](https://genai.owasp.org/llmrisk/llm092025-misinformation/) covers misleading output and overreliance. Preview construction, workload checks and correction tracing above are project recommendations, not certification criteria.
