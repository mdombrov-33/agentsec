# MCP integrations

Establish host/client/server roles, actual transport, negotiated protocol revision, upstream/downstream resources and installed implementation. Revalidate normative requirements against that version before recommending a change.

## Apply the correct authorization model

The checked 2025-11-25 authorization specification concerns HTTP-based transports. It describes authorization as optional and different credential handling for stdio. Do not require an HTTP OAuth flow on every local stdio server. A protocol's optional feature does not remove an application's need to protect private resources. [MCP authorization](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization)

For protected HTTP services, inspect discovery, resource/audience binding, token verification, scopes and the target's per-action policy. Distinguish OAuth authorization from OIDC authentication; follow token format and issuer mechanisms rather than assuming all tokens are JWTs. For a proxy, inspect the client/user consent boundary and downstream credential issuance.

Verify wrong-audience, expired/revoked and insufficient-scope access, plus an intended client action. MCP's guidance prohibits accepting and forwarding client tokens as passthrough to downstream APIs; inspect the actual audience and delegation path. [MCP security guidance](https://modelcontextprotocol.io/docs/2025-11-25/tutorials/security/security_best_practices)

## Isolate sessions and local processes

Distinguish deployment shapes before applying controls:

| Shape | Authority and isolation to inspect |
|---|---|
| Host-launched stdio process | Package/command provenance, inherited credentials, working directory, mounts, process lifetime and network reach |
| Local HTTP service | Listener exposure, Origin/Host handling, DNS rebinding, authentication and cross-user state |
| Remote HTTP service | Transport security, resource/audience binding, client consent, per-action policy and session lifecycle |

For a stdio server, inventory environment **names and permission sources** without dumping secret values. If the server inherits the user's cloud credentials, a narrow advertised tool schema does not constrain what its process can access. Check whether disabling the connector terminates its process and revokes issued credentials.

Trace the authenticated identity independently of session IDs. Inspect shared singletons, resumed streams, temporary files, caches, cleanup and per-user quotas. A session identifier is not a substitute for resource authorization.

For local servers, inspect inherited environment, filesystem mounts, process privileges and network reach. Local HTTP needs appropriate listener exposure, origin/host handling and DNS-rebinding defenses. For remote discovery/fetches, inspect SSRF controls, resolved destinations and redirects. Apply the execution/output reference to dangerous interpreters and fetchers.

Verify two concurrent principals with distinct markers, a resumed/stale session, and cleanup after disconnect. Exercise credential/file access and remote fetch boundaries only inside authorized fixtures. The same-machine location does not establish a least-privilege boundary.

For a stateful remote server, connect two authenticated users, initialize different resources, resume the first stream after the second has made requests, then inspect both the response and effective principal at the resource. Global mutable authorization state can leak across requests even when session IDs differ. Conversely, shared connection pools are not by themselves a cross-user defect if request authorization and state remain scoped.

## Treat discovery and results as untrusted

Inspect tool identity, owner, description, schema, requested permissions and update path. Descriptions and result text can influence the next model step. A tool's advertised behavior is not the policy governing its implementation.

Use the supply-chain reference for selection and update integrity, and identity/tools for semantic authorization. Verify a changed tool description/permission and an injected tool result without executing untrusted installation hooks. Check whether one connected server can induce data flow to another server outside the original task.

Keep a trace of the discovered tool identity/schema, selected arguments, authenticated principal, policy decision and actual effect. A server returning `isError: false` is not evidence that the intended business operation occurred. Inspect the backing record or controlled sink before reporting an effect.

**Limits:** these controls are an integration review aid, not a full protocol conformance suite. Source routes were checked 2026-11-09; actual deployments may use another revision. Proposed signatures, sandbox choices or consent UX beyond normative requirements must be justified by the target's threat model.
