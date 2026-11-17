# Execution and output

Read when model-influenced values reach shell, SQL, templates, file paths, network clients, browsers or exported artifacts. Identify the concrete interpreter and runtime configuration.

## EXEC-01: Validate the resolved operation

Inspect the reachable input, construction API and downstream parser together. Prefer parameterized/typed APIs and semantic constraints for the operation. Separate parsing safety from resource authorization.

| Sink | Incomplete protection | Evidence to inspect |
|---|---|---|
| Subprocess | Passing an argument array avoids shell expansion | Program options, allowed command/function, option termination where supported, working directory and environment |
| SQL | Bound parameters prevent value injection | Selected tenant/rows/columns, stored procedures, dynamic identifiers and caller policy |
| Filesystem | Rejecting `..` checks spelling | Resolved path, symlinks, mounts, special files and race between checking/opening |
| URL fetch | Allowing a hostname checks initial text | Scheme/port, resolved address, redirects, proxy/DNS behavior and actual connection destination |
| Template/eval/deserialization | Model output follows a schema | Whether values still become executable expressions, imports or object constructors |

For paths, prefer APIs that constrain resolution relative to an approved directory and avoid re-opening an attacker-changeable path after validation. For fetchers, enforce destination policy for each connection/redirect; validation and connection must not resolve different addresses. Network egress policy provides a separate layer when client behavior cannot be fully constrained.

Test the real sink with synthetic traversal/symlink, option injection, internal-address redirect or unauthorized row selection as applicable. Observe arguments, opened object, destination or database effect. A dangerous function name alone does not establish exploitability; a safe-looking wrapper can still call it unsafely.

## EXEC-02: Isolate generated computation

Inspect the effective executor: user, mounts, writable paths, environment, credentials, sockets, network/DNS, metadata reach, process privileges and resource limits. A temporary worktree is not a host boundary. A container with a host socket or credential mount may expose the host despite being disposable.

Separate proposal generation from execution and keep control/essential telemetry outside the generated program's authority. Choose isolation for the actual threat: bounded task storage, least privilege, constrained egress and CPU/memory/time/output/disk limits. Decide which artifacts can leave and whether workers survive cancellation. Avoid prescribing one sandbox technology without its configuration and threat assumptions.

Verify controlled access to out-of-scope files, synthetic credentials, a denied destination and a small resource limit. Check persistence after termination. Use approved local fixtures; a process running successfully in a container is not a sandbox-escape assessment. State untested kernel/runtime assumptions.

## EXEC-03: Treat rendering as an effect

Follow returned text through the frontend renderer, sanitizer, Markdown configuration, link handling, image loading and export code. Rendering can cause network traffic without a tool invocation. A remote image URL containing a private marker can disclose it even if script execution is blocked.

Encode/sanitize for the destination. HTML escaping is suitable for text-only output; rich formatting requires a maintained sanitizer and tested allowed elements/attributes/protocols. A CSP or image proxy must be inspected for what it permits, fetches and logs. Escaping an entire HTML fragment is not a drop-in rich-content implementation.

Test active HTML, a remote-image marker and an unsafe URL protocol through the actual renderer when available. Distinguish a raw returned string from observed browser execution/network activity. For spreadsheet exports, inspect formula-leading cells in the generated workbook; HTML escaping does not address spreadsheet interpretation.

Use a legitimate formatting/link/export case alongside the adversarial one. A fix that breaks all rich content needs an explicit product decision. Disclosure authorization belongs to [retrieval/data](retrieval-and-data.md); operation approval belongs to [identity/tools](identity-and-tools.md).

## EXEC-04: Constrain browser and desktop action context

For computer-use agents, inspect the browser profile, active account/origin, application focus, clipboard, downloads and visible permission dialogs. Screenshots and accessibility content are observations, not authorization. A page claiming “session expired” cannot authorize sending credentials elsewhere.

Check whether navigation, redirects, popups or focus changes can alter the recipient/resource between observation, review and action. Re-resolve the target at execution and bind consequential actions to current account/resource state. A coordinate click after a stale screenshot is not evidence that the intended control was activated. Verify the resulting record or destination independently of the page's success text.

Limit browser profiles and mounted user data to the task. Follow downloaded files and clipboard transfers into local interpreters and external destinations. Use a controlled page/window change and a normal unchanged interaction to inspect the boundary. Real desktop actions and external effects remain subject to the task's authorization.

**Sources:** [OWASP LLM05:2025](https://genai.owasp.org/llmrisk/llm052025-improper-output-handling/) covers unsafe downstream interpretation; [Securing Agentic Applications, v1.0](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/) covers code, web and system-operation capabilities. Resolution, computer-use and isolation checks above are project recommendations; use the target runtime's primary API documentation before implementing a fix.
