# n8n Business Flows

This repository contains example n8n workflows for example specific business use case automations.

Each workflow lives in its own folder with:

- A `README.md` describing business value, architecture, and configuration notes.
- An `examples/` directory with sample input/output payloads.

## Current workflows

- `reports-weekly-status/` – **Weekly Business Status Report (local LLM)**
  - Generates a weekly leadership-ready business status report using structured notes and a local LLM, then emails the report.
- `customer-support-digest/` – **Customer Support Digest**
  - Produces a daily/weekly digest of customer support tickets, summarised by an LLM, and delivers it via email or chat.
- `sales-pipeline-enrichment/` – **Sales Pipeline Enrichment**
  - Enriches incoming leads, scores them, and routes them to the appropriate sales channel.
- `churn-risk-alerts/` – **Churn Risk Alerts**
  - Builds a churn watchlist from product usage, billing, and support signals and notifies customer success.
- `multi-channel-campaign-orchestrator/` – **Multi-Channel Campaign Orchestrator**
  - Sets up and synchronises a marketing campaign across email, ads, CRM, and internal comms.
- `invoice-collections-automation/` – **Invoice Collections Automation**
  - Automates reminders, escalations, and reporting for overdue invoices.
- `incident-response-orchestrator/` – **Incident Response Orchestrator**
  - Orchestrates collaboration, ticketing, and communication for high-severity incidents.

Refer to each folder’s `README.md` for workflow-specific details.
