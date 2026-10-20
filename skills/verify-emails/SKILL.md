---
name: verify-emails
description: Verify email addresses and clean email lists with mailfloss. Use when the user asks whether an address is real or deliverable, wants a list of addresses verified or cleaned, or asks about their mailfloss credits, usage or connected email platforms.
---

# Verify emails with mailfloss

Use the mailfloss MCP tools. Verification spends the user's credits, so be
deliberate about it.

## One address, or a few

Call `verify_email` once per address. It costs 1 credit per call, including a
repeat of the same address, so don't verify an address twice in one task.

Report the `status` (`passed`, `undeliverable`, `risky` or `unknown`) and the
`reason`. If the result has a `suggestion`, offer it as a likely typo fix
("Did you mean …?"). Don't change the address yourself.

## A list

1. Tell the user how many addresses you'll verify and that it costs 1 credit per
   address. For a large list, check the balance first with `get_organization`.
2. Wait for the user to confirm, then call `start_batch_verification` with the
   addresses. It returns a job `id`.
3. Poll `get_batch_status` until the job finishes. It's free, so poll at a
   relaxed pace.
4. Read the verdicts with `get_batch_results`. Pass `next` from each page to get
   the following one.
5. Summarize by status and list the addresses that failed. Don't delete or
   unsubscribe anyone unless the user asks.

To stop a running job, call `cancel_batch`. Unprocessed credits are refunded.

## Account questions

- Credits, plan and usage: `get_organization` or `get_usage_report`.
- Connected email platforms and whether their credentials still work:
  `list_integrations`, then `test_integration` on a connection.
- Past verification jobs: `list_jobs`.

## Errors

A failed call returns the mailfloss error envelope. Branch on `error.code`:

- `insufficient_credits`: tell the user they're out of credits and stop.
- `rate_limited`: wait, then retry.
- `invalid_email`: the address is malformed. Report it instead of retrying.
