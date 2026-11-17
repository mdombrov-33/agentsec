# agentsec

A coding-agent skill for reviewing and hardening LLM applications, RAG pipelines and tool-using agents.

It traces untrusted input through retrieval, model calls, tools and stored state. Reviews identify the failing check, the reachable consequence and where to enforce the fix, with code locations and verification steps.

## Install

Run from the project you want to review:

```sh
npx skills add mdombrov-33/agentsec
```

Select your coding agent and choose project or global installation.

## Use

In Codex:

```text
$agentsec Review this repository for AI security issues.
```

Or ask your agent to use `agentsec`. Use it for a code review, threat model, security fix, adversarial test or incident investigation. You can narrow the request to a component, diff or specific failure.

## Coverage

| Reference | Checks |
|---|---|
| [Prompt injection](skills/agentsec/references/context-and-injection.md) | Untrusted instructions, context transformations and guard placement |
| [Retrieval and data](skills/agentsec/references/retrieval-and-data.md) | Tenant/document access, caches, rerankers, fallbacks and disclosure destinations |
| [Memory and compaction](skills/agentsec/references/memory-and-compaction.md) | Write authority, summary dependencies, revocation and later reuse |
| [Identity and tools](skills/agentsec/references/identity-and-tools.md) | Resource authorization, argument validation, approval binding and replay |
| [Human oversight](skills/agentsec/references/human-oversight.md) | Review evidence, misleading previews, decision authority and approval fatigue |
| [MCP](skills/agentsec/references/mcp.md) | Transport-specific authorization, token audiences, sessions and tool discovery |
| [Multi-agent systems](skills/agentsec/references/multi-agent.md) | Delegation scope, message identity, shared state and cancellation |
| [Execution and output](skills/agentsec/references/execution-and-output.md) | Commands, paths, URLs, sandbox access and active rendering |
| [Runtime and recovery](skills/agentsec/references/runtime-and-recovery.md) | Budgets, retries, stop propagation, incident evidence and recovery |
| [Supply chain](skills/agentsec/references/supply-chain.md) | Artifact provenance, model loaders, tool updates and permission changes |
| [Model lifecycle](skills/agentsec/references/model-lifecycle.md) | Training/feedback poisoning, model promotion, privacy claims and rollback |

The [skill entrypoint](skills/agentsec/SKILL.md) selects the workflow and references relevant to the task. Findings distinguish reproduced failures from hypotheses and missing evidence; existing application controls are inspected before reporting a bypass.

## Sources

Built from OWASP publications, the MCP specification, research and practitioner guidance. References cite their sources and distinguish protocol requirements from project recommendations. See [SOURCES.md](SOURCES.md) for editions and attribution.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Check skill discovery with `npm run skills:list`.

[MIT license](LICENSE)
