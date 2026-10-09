# Architecture

The old JSON/Express runtime has been replaced by Cloudflare-native services. D1 is the source of runtime state; R2 stores files; Queues process background jobs; Cron creates scheduled tasks; Workers execute agents and enforce permissions. Frontend never receives provider API keys.

Agent runtime pipeline:
Trigger → Task → Permission check → Prompt + Memory → Provider → Tool loop → Result → Memory/Usage/Logs → Delegation/Approval.

Provider keys are encrypted in D1 using a key held in Cloudflare Secret. Session tokens are HMAC signed using a Worker secret. This is intentionally a single-owner bootstrap model for the first deployment; the schema already contains organization/member isolation for multi-tenant expansion.
