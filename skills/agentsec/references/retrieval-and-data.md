# Retrieval and data

Read for private records, RAG, vector search, caches, provider routing or retained data. Track sensitive bytes through intermediate processors and final output.

## DATA-01: Authorize before each exposure

Trace the actual path:

```text
authenticated subject → query → candidate IDs → content load
→ reranker → model request → answer/citations → cache or derivative
```

Locate trusted subject/tenant attributes and the service applying current record/chunk policy. Model-supplied `tenant_id`, filters or document IDs must not select authority without independent validation. Inspect database row policy and gateway contracts before declaring an application filter missing.

Authorize before release to a model or processor not permitted to receive the data. A reranker may be a separate hosted service: filtering after reranking can already expose text. Search over IDs/scores differs from loading content; determine what the index and processor observe under actual policy.

Inspect direct document lookup, citation resolution, tool retrieval, session reuse and fallback routes. A protected primary retriever does not cover an unfiltered “no results” fallback. Filtering displayed citations cannot retract the model request.

Verify with markers for an allowed record, another tenant and an unauthorized record in the same tenant. Capture each processor's input. Test ID lookup and semantic search separately when both exist. An allowed result must still reach the answer. Report input exposure even if final output omits the marker.

## DATA-02: Scope caches and recheck changing access

Inspect key, value, readers, lifetime and invalidation for cached candidates, documents, answers and session state. `tenant + query` is insufficient when users within a tenant have different ACLs. User-scoped keys still do not handle access revoked after insertion.

Choose policy-aware invalidation, authorization at cache read or a bounded freshness contract suited to the system. Derivatives need source references if current access is checked later. A cached decision without relevant policy/version/lifetime can outlive the grant.

Work this sequence: user A warms a private-result cache; user B asks the same query; A loses access and asks again. Inspect cached bytes sent onward. Repeat on fallback and reference resolution. Apply [memory dependency checks](memory-and-compaction.md) to reused multi-source answers.

## DATA-03: Preserve integrity from ingestion to evidence

Identify who can edit sources, rankings and ACL metadata. Follow parser output, document/chunk IDs, versions, embedding writes and promotion. ACLs in attacker-editable frontmatter are assertions until ingestion binds them to trusted ownership. Parser isolation belongs to [execution and output](execution-and-output.md).

Distinguish an unauthorized index write from malicious text published by an authorized writer. Content remains untrusted after legitimate ingestion. Deduplication, source diversity, freshness or quarantine can limit ranking abuse; select them for the actual writer capabilities and downstream use. Embeddings are not anonymization.

For a consequential claim, inspect cited passage, source authority, currency and retained qualifications. A real authorized citation can fail to support the claim. Trace unsupported evidence to a decision or misleading output; ordinary factual error is not automatically an authorization failure.

Verify metadata tampering, poisoned duplicates, superseded sources and a citation contradicting the conclusion. Preserve legitimate conflicting evidence so the fix does not make disagreement impossible.

## DATA-04: Account for destinations and deletion

Map providers, rerankers, fallback models/regions, logs, errors, exports, caches, vectors and summaries. Compare actual data with approved purpose, destination and retention. Keep secrets in the executor that needs them. Provider routing can change recipients without a code change.

Follow deletion/revocation through source → chunks/vectors → caches → memories → downstream artifacts. Distinguish access revocation, logical deletion, physical erasure and provider commitments. A database deletion does not prove backup or provider deletion. State unavailable evidence explicitly.

**Sources:** [OWASP LLM02:2025](https://genai.owasp.org/llmrisk/llm022025-sensitive-information-disclosure/) covers sensitive-data disclosure; [OWASP LLM08:2025](https://genai.owasp.org/llmrisk/llm082025-vector-and-embedding-weaknesses/) covers vector access, leakage and poisoning. Cache sequences, processor-input checks and derivative policy are project guidance. Recovery is coordinated in [runtime and recovery](runtime-and-recovery.md).
