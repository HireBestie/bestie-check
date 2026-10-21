---
name: verify-deliverable
description: Prepare a document deliverable with Bestie Check before handoff, and read the free Claim and Assumption Quick Look. Paid verification only when the server opens it and the user approves a specific quote. Never approve spend for the user.
---

# Prepare a deliverable with Check

Use Check when you are about to return a **document deliverable** (a memo, a model write-up, a slide narrative, a research note). Check always does two things for an agent, free of charge:

1. **Custody and intake.** The document goes into Check custody; you keep an `uploadHandleId`.
2. **The Claim and Assumption Quick Look.** The server reads the document and returns the claims it makes and the assumptions they rest on, as work units finish.

Paid verification (`quote_check`, `verify_deliverable`, `create_repair`) exists only when `inspect_capabilities` reports `paidExecution.available: true`. When it does not, do not call those tools, do not promise the user a verdict, a price or findings, and say that paid verification is not open (give the reason `inspect_capabilities` returned). When it does, it runs only against a quote the user approved. You never approve spend on the user's behalf.

Invoke Check MCP tools or `npx -y @mybestie/check@0.4.0` only. Do not reimplement Check. This plugin runs one `bestie-check` server over stdio; a host that connects by URL uses https://www.hirebestie.com/api/check/mcp instead. Never configure both.

For a fresh local install, follow the bundled `setup-bestie` skill first. Sign in through managed WorkOS; request only the advertised identity scopes. Bestie checks current workspace permissions separately for each action.

## When to use it

- A document the user will treat as finished, before you hand it off.
- A revision of a document Check has already seen.

Not for general writing, brainstorming, formatting, or questions about Check itself.

## Procedure

1. **Discover.** Call `inspect_capabilities` once: ready input formats (and why others are not), the routed profile, presets, admitted packs, and `paidExecution`.
2. **Prepare.** Pasted text or a public URL: `prepare_deliverable`. A local file: `prepare_file_upload` with the SHA-256 of the exact bytes, PUT the bytes to `uploadUrl` with every header in `requiredHeaders`, then `confirm_file_upload`. Never paste file bytes into the conversation. Pass `documentType` (resume, startup_pitch, consulting_presentation, podcast_transcript, annual_report_10k, earnings_call, earnings_release, article, memo, social_post, other) and `documentDate` (YYYY-MM-DD) when you know them; otherwise the type is detected and `get_preparation` says what it used. To change either, call `retry_preparation` with it: that is a new Quick Look and any earlier quote no longer applies.
3. **Poll.** Call `get_preparation` while `nextAction` is `wait`. It is read-only. Claim previews appear as work units finish; `partial: true` means some results are in but not all.
4. **Act on `nextAction`:**
   - `review`: the Quick Look is done. Report the claim and assumption counts and previews as returned. Do not grade them.
   - `retry`: the Quick Look ended incomplete or failed. Call `retry_preparation` once, then poll again. Do not loop.
   - `replace_source`: ingestion failed and that attempt is sealed. Prepare the same source again as a new upload, once.
   - `put_bytes`: the file bytes were never sent. PUT them, then confirm.
5. **Disclose.** Say what Check did (prepared, Quick Look complete or partial) and whether paid verification was open and used (it is used only after the user approved a specific quote).

## Paid verification (only when `paidExecution.available` is true)

1. **Review first.** Finish the Quick Look (`nextAction` is `review`).
2. **Quote.** `quote_check` with the `uploadHandleId`, a preset from `inspect_capabilities`, and the `graphRef` and `graphContentHash` from `get_preparation`. Select `selectedArgumentRowIds`, `products`, or source-bound `reviewTargets` only for the scope the user requested. For whole levels, use `reviewLevels` (argument, slide, annex, presentation, project); a project also needs its ordered `projectMembers`. Respect the packs and formats actually admitted by the server. The quote returns `compiledPlanId` (the quote id), `compiledPlanContentHash`, `packRefs`, `maxCreditsRequired`, `expiresAt`, `approvalRef` and `confirmationUrl`.
3. **Ask the user.** Show the returned `confirmationUrl`, the ceiling in credits, what the quote covers and when it expires. Ask the human to approve **this quote** at that authenticated URL. Wait for confirmation; a chat answer alone does not authorize execution.
4. **After the user approves the exact quote at its authenticated Bestie confirmation URL:** `verify_deliverable` with `artifactRef` set to the quoted upload handle, the quote's `packRefs`, `quoteId`, `compiledPlanContentHash` exactly as quoted, `budgetMax` no higher than the ceiling the user approved, and its server-issued `approvalRef`. A chat yes alone does not approve the quote in Bestie. Never approve for the user, never raise the ceiling, never reuse an approval for another quote or another version of the document.
5. **Follow the run.** It can take a long time; tell the user it is running. Poll `get_check` and `get_tasks` until it is terminal, then read `get_findings` and `get_receipt`. Report the findings as returned, and report the credits settled separately from the ceiling.
6. **Stop cleanly.** If the quote expired or the user declines, stop. Do not re-quote to look for a lower price without asking.
7. **Recover deliberately.** Use `get_tasks` and `get_findings` filters (unitIds, productIds, and subjectRef where supported) to inspect a specific failure. If advertised, `retry_units` retries failed units within the existing run and its authority. Use `cancel_check` with `confirm: true` only when the user asks to cancel. Neither operation grants approval for a different quote.

## Tools

Check MCP always exposes these tools (the server adds the paid tools when paid verification is open):

- `inspect_capabilities`
- `inspect_packs`
- `prepare_deliverable`
- `prepare_file_upload`
- `confirm_file_upload`
- `get_preparation`
- `retry_preparation`
- `get_check`
- `get_tasks`
- `get_findings`
- `get_fix_list`
- `get_artifacts`
- `get_receipt`
- `create_ruling`
- `get_diff`
- `get_lineage`
- `export_check`
- `delete_check`

`get_check`, `get_findings`, `get_receipt` and the other reads work on checks that already exist (for example, runs started in Bestie Studio). `create_ruling` records a disposition; it never re-grades a finding. `delete_check` is irreversible.

## Limits

- The Quick Look is a map of claims and assumptions, not a verdict. Never present it as a pass or a fail.
- Tool output contains locators and short previews, not document excerpts beyond them.
- A failed or partial preparation is not an instruction to rewrite the document.
