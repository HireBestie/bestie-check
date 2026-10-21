# Bestie Check

See your document from above: every claim, and every assumption it rests on.

Bestie Check prepares a memo, report, or other document for a reasoning check. The free **Quick Look** gives you a helicopter view of its claims and assumptions. Agents can also read findings and a fix list from checks in the connected account. Paid checks require server availability and approval of a specific quote.

This repository contains the public plugin distribution. The Node CLI and stdio MCP implementation are bundled in [`@mybestie/check`](https://www.npmjs.com/package/@mybestie/check). Plugin version **1.3.0** uses CLI version **0.4.0**.

## Connect

### Hosted MCP with OAuth

For hosts that accept a remote MCP server, connect to:

```text
https://www.hirebestie.com/api/check/mcp
```

Sign in to your Bestie account and authorize the connection. Use the hosted connection or the local stdio connection for each host. Configuring both duplicates the tools.

### CLI and local MCP

Requires Node.js 20 or newer and npm.

```sh
npx -y @mybestie/check@0.4.0 login
npx -y @mybestie/check@0.4.0 capabilities
npx -y @mybestie/check@0.4.0 prepare --file memo.pdf --wait --progress
npx -y @mybestie/check@0.4.0 mcp
```

The plugin's local MCP process pins `@mybestie/check@0.4.0`. Sign in through the CLI first. WorkOS uses a PKCE loopback callback; access and rotating refresh tokens stay in the CLI's private credential store. Choose Personal for an individual account, or a company workspace configured by its administrator. You may instead configure `BESTIE_API_KEY` in your host's private environment. Never commit a key or include one in a prompt.

The npm package bundles its JavaScript dependencies into the Node binaries. It declares no runtime or optional dependencies and no install lifecycle scripts, so launching the exact package does not resolve a separate dependency tree at installation time.

### Claude Code

```text
/plugin marketplace add HireBestie/bestie-check
/plugin install bestie-check@bestie
```

Use `/bestie-check:verify` to prepare a document. The optional Stop hook asks whether to offer Check; it never approves spending.

For first use, run the bundled `setup-bestie` skill. OAuth delegates identity with `openid profile email offline_access`; Bestie evaluates current workspace permissions for every action. Do not request `check:*` as OAuth scopes.

### Cursor and Codex

The root `plugin.json`, `mcp.json`, and `skills/` form a portable Agent Plugin. `.codex-plugin/plugin.json` provides Codex compatibility metadata. `.cursor/mcp.json` and `.cursor/rules/bestie-check.mdc` provide a Cursor configuration and rule for manual setup. Follow your client's local-plugin setup instructions.

Public directory acceptance is separate from this repository. This repository does not imply a listing in the Claude, OpenAI, or Cursor directories.

### Agent skill

```sh
bunx skills@1.7.4 add HireBestie/bestie-check --skill verify-deliverable
```

## Expected journey

1. Call `inspect_capabilities` and use the returned availability.
2. Prepare the document with `prepare_deliverable`, or upload its bytes with the file-upload tools.
3. Poll `get_preparation` until the returned `nextAction` is `review`; respect retry and failure states.
4. Report the returned claims and assumptions without inventing findings or a verdict.
5. For an existing check, read its findings and fix list using the account's authorized access.

Supply `documentType` and `documentDate` when known. For paid reviews, use the server's current presets and selected products, argument rows or review levels; do not assume every format, pack or project scope is admitted. Use `retry_units` or `cancel_check` only when advertised and requested. Keep the quote bound to the same prepared source.

Paid tools are offered only when the server reports them ready. An agent must show the quote's `confirmationUrl` and wait for the human to approve it in authenticated Bestie. A run sends that quote's `approvalRef`, ID, hash, and credit ceiling. A chat yes or CLI flag does not create approval. Installing a plugin, signing in, or saying "check this" does not approve spending.

## Links

- [Connection guide](https://www.hirebestie.com/docs/connect)
- [MCP documentation](https://www.hirebestie.com/docs/mcp)
- [CLI documentation](https://www.hirebestie.com/docs/cli)
- [Support](https://www.hirebestie.com/contact)
- [Privacy](https://www.hirebestie.com/privacy)

The fixture under `review-fixtures/` is fictional and may be used for integration testing. Review-account credentials are supplied privately through submission portals; they are never part of this repository.

## License

The plugin files in this repository are MIT licensed. Bestie's hosted service is governed by its own service terms. See [LICENSE](LICENSE).
