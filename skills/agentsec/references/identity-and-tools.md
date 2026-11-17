# Identity and tools

Read when the model can call tools, delegate credentials, request approval or cause a business effect. Establish the permitted task before judging an operation.

## AUTH-01: Carry initiating authority to the resource

Follow initiating user/service, application workload, agent task/worker and target resource separately. Inspect where tenant, subject, resource, action, purpose and expiry are established. A workload token can authenticate the caller while saying nothing about which customer's invoice it may change.

Use a concrete operation record. An invoice task might permit reading tenant A's invoices and drafting a payment, while forbidding submission or vendor-account changes. The representation can be a server-side task record or bounded capability. The model may propose an invoice ID; the executor resolves ownership and current policy independently.

Inspect resource middleware, database policy and executor callers. Check alternate routes through generic HTTP/SQL/shell tools, workers, retries and callbacks. Broad credentials are an exposure, but resource policy can still provide complete mediation. Unavailable deployment policy is an evidence gap, not proof of bypass.

Verify wrong tenant/resource, insufficient operation scope, expired/revoked grant and the intended operation. Observe the system of record. Delegation belongs to [multi-agent systems](multi-agent.md); protocol token requirements belong to [MCP](mcp.md).

## AUTH-02: Validate business meaning and composition

JSON types do not establish recipient ownership, refund eligibility, cumulative amount, permitted columns or transaction state. Read business rules and determine whether validation runs at the component committing the effect.

Prefer `create_refund_draft(order_id, amount)` with a distinct commit path when that separation fits the product. A generic internal API tool can bypass the narrow tool if both receive ambient write credentials. Removing an exposed primitive can be simpler than teaching the planner every prohibited use.

Check combinations: a permitted private read followed by a permitted external message may violate the task's disclosure purpose. Constrain data destinations or the capability set. Coexisting reads and outbound tools are not a demonstrated leak; establish a reachable request passing existing controls.

Test a schema-valid unauthorized amount/resource and a prohibited sequence. Repeated calls must respect cumulative business limits. Distinguish monetary totals, action counts and model-token budgets; [runtime](runtime-and-recovery.md) owns resource accounting.

## AUTH-03: Bind approval to the committed effect

Trace proposal → reviewer view → approval record → queue → executor → commit. Determine who can alter each representation. Use [human oversight](human-oversight.md) when assessing what the reviewer sees and whether source evidence is available. This section owns the binding between that decision and execution.

For an approved refund, inspect the binding of:

```text
approver + initiating subject + tenant + operation
order/account + recipient + amount/currency
resource version + expiry + approval state + operation/idempotency ID
```

Choose fields according to the effect; this is not a universal wire schema. Canonicalize before approval so preview and execution interpret the same values. Store an immutable operation server-side or verify an authenticated binding. A hash alone does not authenticate an approver, enforce expiry or prevent replay.

Before commit, check current policy, grant validity, operation binding and relevant resource version. Make approval consumption and local state changes atomic where possible. A single-use flag checked outside the transaction can be raced by two workers. For an external API, use its idempotency semantics and durable operation/outbox state; local approval consumption cannot make an arbitrary remote effect atomic.

Worked sequence:

1. Approve a refund to the order's existing customer account.
2. Change recipient or amount while keeping the approval ID.
3. Dispatch the changed request, then retry the original request twice.
4. Inspect the ledger: changed arguments must not commit; the approved unchanged operation may commit once.

Also test resource mutation after approval, reuse against another operation, another principal, expiry and revocation before execution. A separate unchanged case must succeed. Sequential replay tests do not establish concurrent safety.

## AUTH-04: Handle cancellation and ambiguous effects

Check current task cancellation/revocation before dispatch and at the last enforceable commit boundary. Inspect policy-storage failure, executor crash and an external timeout after possible commit. Retrying an ambiguous non-idempotent effect can duplicate it.

Define permitted recovery: discard an uncommitted draft; reconcile a submitted payment or use a compensating transaction. Report unavoidable in-flight effects. The stop path and evidence collection belong to [runtime and recovery](runtime-and-recovery.md).

## AUTH-05: Keep credentials out of model-visible data

Locate the credential store and every injection point: executor environment, connector initialization, tool arguments/results, errors, traces, screenshots and generated artifacts. Prefer an opaque tool capability to a raw API key in context. Issue the narrowest audience/operation scope and practical lifetime, inject credentials only where needed, and revoke child grants when the parent task loses authority.

Inspect redaction at collection and error paths, not only the normal response. A successful tool can hide a secret in its failure traceback on the next call. Verify with synthetic markers, observing model requests and controlled telemetry; never dump production environment values to demonstrate a leak. For suspected exposure, identify affected credentials and consumers and coordinate revocation through the incident workflow.

**Sources:** [OWASP LLM06:2025](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) distinguishes excessive functionality, permission and autonomy. [ASD, Agentic AI Harnesses, September 2026](https://www.cyber.gov.au/publication/agentic-ai-harnesses) places permission and execution controls in the harness. Operation binding, secret handling and verification sequences above are project recommendations, not a mandated IAM product or cryptographic scheme.
