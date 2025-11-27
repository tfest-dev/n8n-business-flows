# Multi-Channel Campaign Orchestrator

This workflow coordinates a marketing campaign across multiple channels (email, ads, CRM, and internal comms) and keeps everything in sync.

## Business value

- Reduces manual setup work across tools when launching campaigns.
- Ensures messaging, timing, and targeting stay consistent across channels.
- Provides a central place to track campaign status and issues.

## How this n8n flow works

This example is implemented in n8n as a minimal, connector-agnostic template. It focuses on the data flow and fan-out/fan-in pattern; you swap the placeholder Code nodes for your actual tools.

1. **Trigger**
   - `Manual Trigger (dev)`: used for demos and local testing.
   - Deployment supplements this with a Webhook or Cron trigger.

2. **Set Campaign**
   - `Set` node that initialises a single item with campaign metadata:
     - `name`, `audience_segment`, `start_date`, `end_date`, `budget_usd`, `channels`, `goals`.
   - Deployment feeds this from a form, PM tool, or CRM

3. **Validate & Normalise**
   - `Code` node that:
     - Verifies required fields are present and that `budget_usd` is a positive number.
     - Parses the `channels` and `goals` JSON strings into proper objects.
     - Throws a clear error if validation fails (stopping the run).

4. **Parallel channel setup (fan-out)**
   - From `Validate & Normalise`, the flow branches to four `Code` nodes that run in parallel:
     - **Setup Email Campaign** – attaches an `email` object (enabled flag, placeholder segment/campaign IDs).
     - **Setup Ads** – attaches an `ads` object with placeholder IDs and the campaign budget.
     - **Setup CRM Campaign** – attaches a `crm` object with a placeholder campaign ID and tagged account count.
     - **Notify Internal Comms** – attaches an `internalComms` object with a Slack channel name and placeholder message link.
   - Deployment flow replaces these `Code` nodes with HTTP Request or native app nodes (email platform, ads platform, CRM, Slack/Teams, etc.).

5. **Fan-in & status aggregation**
   - Three `Merge` nodes bring the branches back together:
     - `Merge Email + Ads` combines the email and ads outputs.
     - `Merge Campaign + CRM` combines the campaign and CRM outputs.
     - `Merge Full Campaign` combines the prior merge result with the internal-comms path into a single enriched campaign object.

6. **Build Summary**
   - Final `Code` node (`Build Summary`) generates a `summary_markdown` string that:
     - Lists the campaign name, audience, dates, and budget.
     - Summarises which channels are enabled and confirms that placeholder entities were "created".
   - You can send this summary to email/Slack or log it to a database/sheet for audit.

## Deployment changes

For a client-specific implementation:

- Change the **trigger** to a Webhook or Cron schedule tied to your campaign approval process.
- Replace the four channel `Code` nodes with:
  - Email platform node(s) to create/update segments and campaigns.
  - Ads connector(s) to create ad sets/campaigns with real targeting.
  - CRM connector(s) to create a campaign object and tag accounts.
  - Slack/Teams node(s) to post into your real `#campaigns` channel.
- Extend `Build Summary` to include deep links (platform URLs, IDs) returned from those connectors.

## Concurrency & error handling

- The email, ads, CRM, and internal comms branches run in parallel immediately after `Validate & Normalise`.
- Deployment would add per-branch error handling:
  - If one channel fails (e.g. ads API down), log the error and continue with other branches.
  - Surface partial failures in a final Slack/email notification and/or monitoring system.

## Examples

- `examples/sample_input.md`: example new campaign payload.
- `examples/sample_output.md`: example of the aggregated campaign setup summary.
