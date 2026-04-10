# Dev Weekly — 2026-04-06

_Source: Granola note “Dev Weekly” (Apr 6, 2026, 17:00–17:30 SGT)_

## TL;DR
CBV2 sign-in is working; Codex-driven app generation is in progress (reasoning UI still buggy). Connector migration from ChatChat is underway, with a key architectural requirement: **all CBV2 coding tasks must run via persistent background workers**. In parallel, the team is converging on an **OpenClaw-as-sub-agent** model (scoped connector access, memory owned by ChatChat), migrating shell execution from Chat Completions → Responses API, and exploring **proactive event streaming** from connectors. Credit-based billing is a shared launch dependency.

## Highlights
### CBV2 progress
- Authentication works with existing user records (everyone can sign in)
- App-building flow being implemented using Codex
  - LLM app generation works
  - Reasoning-step display still buggy
- Connector migration from ChatChat
  - Server-side code exists, needs testing/validation
- Non-negotiable requirement: **persistent background workers** for all coding tasks
  - Target behavior modeled after Codex app on Mac/Windows
- CBV2 must ultimately support **all connectors** currently supported in ChatChat
- OpenClaw shell integration needed in CBV2 (dependent on ChatChat work)
- GitHub connector is now considered straightforward to add

### OpenClaw / VM architecture
- Proposed model: OpenClaw runs as a **sub-agent** inside the main ChatChat agent
  - ChatChat owns memory + orchestration
  - OpenClaw is “stateless” beyond the task it’s given
- Security posture: agent access is **scoped to configured connectors**
  - e.g., 6 connectors configured → exposure limited to those 6
  - Users can configure “contexts” (e.g., work-only)
- Shell replacement in progress
  - Current shell uses Chat Completion API
  - Shell needs Responses API
  - Risk: API switch could impact existing functions → ensure backward compatibility

### Proactive event streaming (vision)
- Move from reactive (prompt → response) to **proactive connector-driven events**
- Poll connectors on a schedule (e.g., every 5 minutes) and push notable events
- Example use cases
  - Unanswered emails flagged automatically
  - Calendar + traffic detects risk of being late → suggests booking Uber
- Research owned by Ping An; ties into connector orchestration layer
- Relevant to both ChatChat and CBV2

### Other updates
- Jingbi
  - Finished Chat mobile onboarding screens + styles
  - Moving to Chatandbuild website; aiming to publish by tomorrow
- Ping An (via Christel)
  - Completed multiple Google connectors + Terra health connectors
  - Drafted Google Slides architecture
  - Working on Granola MCP connector + more Google connectors (Maps/Meet/Places)
  - Working on credit-based billing
- Credit-based billing system
  - Needed for both ChatChat + Chatandbuild launch
  - Plan should unify “coding” + “agent usage” credits
- Cloudflare D1
  - Noted as an alternative to Supabase (Lovable-style serverless DB)
  - Not an immediate priority

## Action items / next steps
### Zhiwen
- Wrap up CBV2 app-building process
- Test + validate migrated connectors
- Enforce persistent background worker requirement for coding tasks
- Add OpenClaw shell integration once ChatChat work is ready
- Add GitHub connector to CBV2

### Zhangtao
- Complete shell replacement (Chat Completion → Responses API) without regressions
- Work on VM/Docker setup for OpenClaw in parallel
- Develop a simple external explanation of the ChatChat + OpenClaw architecture

### Ping An
- Post weekly update in Slack
- Continue proactive event streaming + connector orchestration research
- Progress Granola MCP connector + remaining Google connectors (Maps/Meet/Places)
- Implement credit-based billing system

### Jingbi
- Publish Chatandbuild website by tomorrow
- Resume ChatChat mobile app work after site launch
