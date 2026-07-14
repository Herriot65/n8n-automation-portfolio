# Form Submission → Instant Email Notifier

## Problem
Many businesses collect leads and inquiries through website forms, but nobody is
actively watching the form's inbox. A submission can sit unread for hours until
someone manually checks — by which point a slow response has already cost the
business the lead, or an urgent request has gone unhandled. Manual monitoring
doesn't scale and isn't reliable.

## Solution
An automated workflow that triggers the instant a form is submitted, extracts the
submitted data, and sends a formatted email notification to a designated recipient
within seconds — no manual checking required. The email body is dynamically
populated from the form fields (not a static template), so the recipient sees the
actual submission content immediately.

## Architecture
Form Trigger (On form submission) → Send Email (Gmail)
- **Form Trigger** captures the submission (name, email, message) as structured JSON.
- **Send Email** dynamically injects the submitted fields into the email subject/body
  using n8n expressions (e.g. `{{ $json['Full Name'] }}`), so each notification
  reflects the actual submission rather than a generic message.

## Setup Instructions
1. Import `workflow.json` into your n8n instance.
2. Connect a Gmail (or SMTP) credential under the **Send Email** node.
3. Set the recipient address in the **To** field.
4. Activate the workflow to get a production form URL, or use the test URL during setup.

## Notes
- Recipient is currently fixed to a single address — appropriate when one person
  owns the inbox. For teams needing message routing by type/urgency, this can be
  extended with conditional logic (If/Switch) to route to different recipients.
