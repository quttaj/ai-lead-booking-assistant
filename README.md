# AI Lead & Booking Assistant

An independent portfolio prototype for processing appointment requests for a beauty salon.

Built by **Ivanna Poltavets**, 2026.

## Overview

The workflow connects a Google Form and Google Sheets with Make, OpenAI, Gmail and Telegram. It creates a structured AI summary and a draft reply, assigns a processing priority, records the result and routes notifications to the client and salon owner.

This repository documents the prototype and its recorded demo checks. It is not a ready-to-import deployment package.

![Make workflow](assets/workflow.jpg)

## My contribution

- Built the Make scenario and connected the services.
- Configured structured AI output, request priority and processing statuses.
- Added duplicate checks, basic email-format validation, logging and error/retry routes.
- Prepared and ran demo scenarios to check normal processing and exception handling.

## Workflow

1. A Google Form collects an appointment request and stores it in a spreadsheet.
2. Make reads new requests from Google Sheets and checks processing state.
3. OpenAI produces a structured summary and draft response; the scenario parses the result.
4. The scenario records priority, status and processing results.
5. Gmail sends an acknowledgement when appropriate; Telegram notifies the owner.
6. Duplicate requests and failure paths are handled separately.

Google Sheets is the visible trigger in the Make workflow. Form submissions enter that sheet upstream.

## Demo outputs

### Client acknowledgement

The email confirms receipt of the request, not the appointment itself. The screenshot uses an error-test name because it comes from a demo scenario.

![Client acknowledgement](assets/client-email.png)

### Owner notification

The example highlights a conflict between the selected service and the client's message. Contact rows and other conversations have been excluded from this image.

![Owner notification excerpt](assets/owner-notification.png)

### Recorded checks

![Demo test table](assets/test-results.png)

See [TESTING.md](TESTING.md) for the nine cases visible in the supplied evidence. The table uses example email addresses; “Sent” is a recorded demo outcome, not independent proof of delivery to those example addresses.

## Scope and limitations

- This is a request-processing prototype, not a real-time booking calendar.
- A salon employee must confirm availability and the final appointment.
- Email-format validation does not verify mailbox existence or deliverability.
- The prototype was configured for scheduled runs at 15-minute intervals. Scheduling depends on the deployed Make configuration and account plan.
- Deployment requires the client's connected accounts and configuration.
- This documentation does not claim production traffic, paid clients or measured business savings.

## Privacy and reproducibility

The live Make blueprint is intentionally excluded because it may contain email addresses, chat IDs and spreadsheet IDs. No credentials or connection exports are included. A sanitised blueprint and exact setup instructions can be added later after reviewing all modules, prompts and sample data.

The public evidence demonstrates workflow structure and selected outputs. It does not include every configuration value or an independently reproducible end-to-end test environment.

## Tools

Make · OpenAI API · Google Forms · Google Sheets · Gmail · Telegram
