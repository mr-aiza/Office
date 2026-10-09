# 🏢 Virtual AI Office 2.0

Cloud-native AI Company OS built for GitHub + Cloudflare.

## Stack
- React + React Three Fiber / Three.js frontend
- Cloudflare Workers API + Agent Runtime
- D1 database
- KV cache/session support
- R2 document storage
- Queues for background Agent jobs
- Cron Triggers for schedules
- Cloudflare Secrets for sensitive configuration
- GitHub Actions for CI/CD

## Deploy without installing Node locally
GitHub Actions performs build/deploy. Create the Cloudflare resources first:

1. D1: `virtual-ai-office`
2. KV namespace
3. R2 bucket: `virtual-ai-office-files`
4. Queue: `virtual-ai-office-jobs`
5. Put IDs into `worker/wrangler.jsonc`.
6. Apply migrations with Wrangler from GitHub Actions or Cloudflare's build environment.
7. Set Worker secrets: `ADMIN_PASSWORD`, `SESSION_SECRET`, `PROVIDER_ENCRYPTION_KEY`. Optional `GITHUB_TOKEN` enables the GitHub tool.
8. Add GitHub repository secrets `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`.

## Important
The current runtime uses a signed session token and encrypts provider API keys before D1 storage. API keys are never returned to the browser. Sensitive tool operations create approval requests.

The frontend can point at the Worker with `VITE_API_URL`. For Pages builds, set that as a Pages/GitHub Actions environment variable to the Worker API URL.

## Agent capabilities
Agents have identity, prompts, permissions, memory, tasks, scheduling, delegation, execution traces, approvals, provider/model selection, cost/usage records, workflows, logs and projects.

## No fake tools
Implemented core tools are calculator, controlled HTTP, memory and delegation. GitHub is guarded behind a Worker secret and approval. Email/browser/full document ingestion need their external credentials/processing pipeline before they should be exposed as enabled tools.
