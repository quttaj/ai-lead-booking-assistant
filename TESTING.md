# Demo Testing

## Summary

The Portfolio Demo test log contains **15 scenarios, all marked PASS**.

The scenarios check request analysis, priorities, review statuses, basic validation and repeat-processing protection. A successful test does not always mean a request reaches `Processed`: routing an incomplete request to `Needs review` or blocking an empty required field can be the expected result.

The results below reflect the recorded demo tests. Some scenarios inspect different aspects of the same request, so 15 scenarios does not mean 15 unique leads.

## Test results

| # | Scenario | Recorded behaviour | Priority | Status / outcome | Client email | Owner notification | Result |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Standard request | AI summary and reply draft created; request processed | Medium | Processed | Sent | Sent | PASS |
| 2 | Urgent request | High priority assigned automatically | High | Processed | Sent | Sent | PASS |
| 3 | Missing phone | Request processed using email as the contact method | Medium | Processed | Sent | Sent | PASS |
| 4 | Missing email | Required field blocked submission before it reached Make | — | Blocked by form | Not sent | Not sent | PASS |
| 5 | Conflicting service | Conflict between the selected service and comment identified; clarification needed | High | Needs review | Sent | Sent | PASS |
| 6 | Very short message | A usable summary and reply draft produced from a minimal comment | Medium | Processed | Sent | Recorded | PASS |
| 7 | Long message | Detailed request summarised without inventing price or availability | Medium | Needs review | Sent | Recorded | PASS |
| 8 | Invalid email format | Client acknowledgement skipped; owner still notified | Low | Needs review | Not sent | Sent | PASS |
| 9 | Duplicate request | Existing Request ID skipped; no repeated notifications | Medium | Skipped duplicate | Not repeated | Not repeated | PASS |
| 10 | Unknown date | Missing preferred date and time identified for clarification | High | Needs review | Sent | Sent | PASS |
| 11 | Multiple services | Additional services recognised; staff clarification required | Medium | Needs review | Sent | Recorded | PASS |
| 12 | Off-topic message | Phone-repair enquiry recognised as unrelated to the selected salon service | Medium | Needs review | Sent | Recorded | PASS |
| 13 | Nearest-day request | High priority assigned and owner alerted | High | Processed | Sent | Sent | PASS |
| 14 | Price-only inquiry | No price invented; missing date and time identified | Medium | Needs review | Sent | Recorded | PASS |
| 15 | Empty comment | Request preserved; missing date and time identified | High | Needs review | Sent | Sent | PASS |

## Interpreting the results

### AI drafts and client emails are different outputs

References to a reply or clarification describe the AI output saved for staff review. The automatic client email is a standard acknowledgement, not the AI-generated reply draft.

### Priority is rule-based

Make assigns priority from the urgency selection. The test-case name, missing information or AI summary does not independently determine priority.

A request can be `High` priority and still require review.

### Notification labels preserve the original log

- `Sent` records a successful send action in the demo log; it does not establish inbox delivery or that the recipient read the message.
- `Recorded` is retained exactly as written in the source log and is not treated as equivalent to `Sent`.
- `Not sent` means the notification was not sent for that case.
- `Not repeated` means the duplicate check prevented another notification.

Although the workflow is configured to notify the owner for all normally processed requests, the test log does not mark every owner notification as `Sent`.

### Duplicate protection has a specific scope

The duplicate case reran an existing Request ID. It demonstrates protection against repeated processing of the same spreadsheet row.

It does not demonstrate detection of similar requests submitted as separate rows, or duplicate-free recovery from every partial execution.

### Form validation is separate from Make processing

The missing-email case tests Google Forms validation: the request is blocked before Make receives it.

The invalid-email-format case tests how the workflow handles a malformed address present in a request.

### Status and test outcome are separate

`Blocked by form` and `Skipped duplicate` describe test outcomes. They should not be confused with the normal processing statuses `Processing`, `Processed`, `Needs review` and `Error`.

## Additional functional checks

The project notes also report checks of:

- Structured AI JSON output.
- Missing-information detection.
- Gmail and Telegram sending.
- Priority and status assignment.
- Writing results to the correct spreadsheet row.
- Repeat-processing protection.
- Error-handling routes.

The 15-row table does not list each technical failure and recovery check separately. These additional checks are therefore not counted as extra numbered PASS cases here.

## Error-handling review

The current implementation includes routes for OpenAI failures, JSON-parsing failures, Gmail failures and Telegram failures.

Telegram notification failures have a fallback email route to the owner. Failed executions are retained in Make for inspection and controlled recovery.

Two known issues remain:

| Issue | Current impact | Follow-up |
| --- | --- | --- |
| JSON and Gmail handlers write `Error message` literally | The log does not preserve the actual technical error for those paths | Map the real error value and retest |
| JSON-parsing failure has no separate owner alert | Staff may need to discover the failure through the spreadsheet or Make execution history | Add an owner notification and retest |

The 15 PASS results describe the listed request scenarios. They do not mean every technical failure path is fully covered or issue-free.

## Follow-up tests

After correcting the known issues, document separate results for:

- OpenAI request failure.
- Malformed or unparseable AI output.
- Gmail acknowledgement failure.
- Telegram failure and successful fallback email.
- Failure of the fallback email itself.
- Recovery of an incomplete execution after one notification has already succeeded.
- An email address with valid syntax but an unreachable mailbox or domain.

These are follow-up checks, not additional PASS claims.

## Evidence

- [Workflow diagram](assets/workflow.jpg)
- [Test-log screenshot — first nine cases](assets/test-results.png)
- [Client acknowledgement](assets/client-email.png)
- [Owner notification excerpt](assets/owner-notification.png)

Client contact details are omitted from the results table. Example email addresses in the screenshot are used for presentation and do not demonstrate delivery to those addresses.
