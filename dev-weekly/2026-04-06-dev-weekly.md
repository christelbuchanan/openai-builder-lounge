# Dev Weekly — 2026-04-06

Source: Granola note “Dev Weekly” (created 2026-04-06, updated 2026-04-07)

## CBV2 progress
- Authentication works with existing user records — all users can sign in to CBV2.
- App-building process is being implemented using Codex.
  - LLM app generation works, but the “reasoning steps” display is still buggy.
- Connectors are being migrated from ChatChat.
  - Server-side code is already written but not yet tested.
- **Key requirement:** all coding tasks in CBV2 must use **persistent background workers**.
  - Reference behavior: Codex app on Mac/Windows.
- All connectors supported in ChatChat must be supported in CBV2.
- OpenClaw shell integration is needed in CBV2 (Zhangtao leading in ChatChat; CBV2 to assist).
- GitHub connector to be added — noted as easier to implement now than previously.

## OpenClaw / VM architecture
- Plan: OpenClaw runs as a **sub-agent** within the main ChatChat agent.
  - Memory is managed entirely on ChatChat’s end.
  - OpenClaw sub-agent handles only the assigned task via API.
  - OpenClaw sub-agent holds **no persistent memory/data** itself.
- Differentiator vs standalone OpenClaw: access is **scoped to configured connectors only**.
  - Example: if an agent has 6 connectors, exposure is limited to those 6.
  - Users can configure context (e.g., “work-only” = productivity connectors + productivity memory).
- Shell replacement:
  - Current API uses Chat Completions; shell requires the Responses API.
  - Migration in progress.
  - **Risk:** API switch impact on existing functions is unclear; Zhangtao to ensure backward compatibility.

## Proactive event streaming
- Current AI is reactive (prompt → response); goal is proactive event streaming from connectors.
- Connectors polled on a schedule (e.g., every 5 minutes) to push events to the agent.
- Example use cases:
  - Unanswered emails flagged proactively.
  - Google Maps + Calendar detects upcoming meeting + bad traffic → suggests booking Uber.
- Ping An researching this; will tie into the connector orchestration layer.
- Relevant to both ChatChat and CBV2.

## Other updates
- Jingbi: completed Chat mobile app onboarding screens + onboarding styles; now moving to Chatandbuild website — expects to publish by tomorrow.
- Ping An (via Christel summary):
  - Completed multiple Google connectors, Terra health connectors, and a Google Slides architecture.
  - Working on Granola MCP connector and additional connectors (Google Maps, Google Meet, Google Places).
  - Also working on a credit-based billing system.
- Credit-based billing system needed for both ChatChat and Chatandbuild launch — unified plan covering coding and agent usage.
- Cloudflare D1 flagged as worth evaluating as an alternative to Supabase (architecture used by Lovable for auto-deployed backends) — not an immediate priority.

## Next steps / owners
### Zhiwen
- Wrap up app-building process in CBV2 today.
- Test and validate migrated connectors.
- Ensure all CBV2 coding tasks use persistent background workers.
- Add OpenClaw shell integration to CBV2 once Zhangtao’s ChatChat work is ready.
- Add GitHub connector to CBV2.

### Zhangtao
- Complete shell replacement (Chat Completions → Responses API) — ensure no regression on existing functions.
- Work on VM/Docker setup for OpenClaw in parallel (don’t wait for shell work to finish).
- Continue thinking through how to articulate the ChatChat + OpenClaw architecture simply for external audiences.

### Ping An
- Drop weekly update in Slack.
- Continue research on proactive event streaming + connector orchestration.
- Progress Granola MCP connector and remaining connector list (Google Maps, Meet, Places).
- Implement credit-based billing system.

### Jingbi
- Publish Chatandbuild website by tomorrow.
- Continue ChatChat mobile app work after site launch.

---

Granola link: https://notes.granola.ai/t/7ba5a2ef-8b70-48d4-88be-7c21403bb51c
