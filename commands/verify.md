---
description: Prepare a document deliverable with Bestie Check and read its Claim and Assumption Quick Look (free; paid verification only when the server opens it)
---

Follow the `verify-deliverable` skill.

1. `inspect_capabilities` once.
2. Prepare the document (`prepare_deliverable`, or `prepare_file_upload` → PUT → `confirm_file_upload`). Keep the `uploadHandleId`.
3. Poll `get_preparation` until `nextAction` is not `wait`. On `retry`, call `retry_preparation` once.
4. Report the Quick Look as returned: claim and assumption counts and previews, and whether it is partial.

Call `quote_check`, `verify_deliverable` or `create_repair` only when `inspect_capabilities` reports `paidExecution.available: true`, the user asked for a paid check, and the user approved the specific quote (follow the paid section of the skill). Otherwise say paid verification is not open. Never approve spend for the user. Do not grade the Quick Look.
