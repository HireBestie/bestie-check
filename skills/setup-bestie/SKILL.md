---
name: setup-bestie
description: Connect Bestie Check, choose a Personal or company workspace, and verify access without preparing documents or spending units.
---

# Set up Bestie Check

Use one connection per host. The plugin includes a local stdio server; if the host already connects to https://www.hirebestie.com/api/check/mcp, use that remote connection instead of enabling a duplicate.

1. For the bundled local server, run `npx -y @mybestie/check@0.4.0 login`. For a remote connection, use the host's Connect action and the canonical HTTPS URL above.
2. Let the person sign in and consent in WorkOS. Never ask them to paste a password, token, or verification code into chat. OAuth requests the advertised identity scopes: openid, profile, email, offline_access. Check action names are resource permissions, not additional WorkOS OAuth scopes.
3. An individual can choose Personal. Choose a company workspace only if its administrator configured it for Check and the person intends to use that company's access and funding. A workspace-binding refusal requires an operator; repeated sign-in does not register a company workspace.
4. Call `inspect_capabilities` (CLI: `npx -y @mybestie/check@0.4.0 capabilities`) and report actual input readiness, paid readiness and spending policy. This is read-only. If the host requires it, reload its MCP connection after login so tools are discovered.
5. Readiness is not approval to spend. A requested document follows the verify-deliverable skill; a paid run requires the human to approve its exact quote at the returned authenticated Bestie confirmation URL. Never mint an approval or change workspace membership to bypass a refusal.

If an old install reports credential_issuer_retired, update to this pinned CLI version and run login again. For other failures, use `npx -y @mybestie/check@0.4.0 doctor --json` and report the error without exposing credential contents.
