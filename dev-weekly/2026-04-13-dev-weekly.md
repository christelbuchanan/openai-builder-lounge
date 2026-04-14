# Dev Weekly — 2026-04-13

Source: Granola note “Dev Weekly” (2026-04-13 17:00–17:30 SGT)

## TL;DR
CBV2 is progressing well (auth + research mode + task routing + file tree drawer + previews). Connector migration is underway but needs per-connector configuration (redirect/web URLs). UI polish is in flight (new designs, font, mobile sidebar/zoom fixes). Infra work focuses on runtime status/stop button correctness, legacy HTTP+WS integration, and near-complete OpenClaw VM integration to replace OpenAI computer-use. Mobile work resumes with iOS first, then Android shortly after.

## Progress / Demos
### CBV2 development
- Auth working properly.
- Research mode functional, including sidebar navigation.
- Action buttons working (e.g., conduct research, design website).
  - URLs update in a ChatGPT-like format.
  - Routes to the appropriate task types.
- Connectors: backend is connected, but each connector still needs individual configuration.
  - Redirect URLs + web URLs still need to be set up.
- File tree functionality complete.
  - Appears as a drawer when file generation completes.
  - Preview is available; scroll issues are being fixed.
- Split view planned.
  - Pattern: full-screen reasoning first, then split for preview (similar to Cursor).
- Background jobs supported.

## UI/UX
- Jingbi’s design work is landing well.
  - Landing page layout updates in progress.
  - “Circular Standard” font to be implemented across Chat.
- Current issues being resolved:
  - Layout conflicts from simultaneous updates.
  - Sidebar display problems on mobile.
  - Zoom functionality needs improvement.
- Future enhancements discussed:
  - Pinned builds and pinned research display.
  - Tile view for built apps (Cursor-like).
  - Integration-based examples for build prompts.

## Infrastructure / Platform
- Runtime status + stop button fixes (Zhangtao).
  - Bug: stop button inactive when tasks are queued but not running.
  - Working on legacy HTTP + WebSocket integration.
- OpenClaw VM integration nearly complete.
  - Replace OpenAI computer-use with OpenClaw API.
  - Expected to improve accuracy vs screenshot-only analysis.
  - Testing pending locally.

## Mobile
- iOS app work continues this week (after CBV2 landing page work).
- Android expected ~2–3 days after iOS completion.
- Plan: TestFlight testing before App Store asset preparation.

## Next steps / Owners
- Christel
  - Important meeting tomorrow 6 PM.
  - Status update needed tonight and by noon tomorrow.
- Zhiwen
  - Deploy changes to staging for testing today.
  - Multi-cursor instance setup to speed connector migration.
- Team
  - Continue Chat platform improvements alongside CBV2.

---

Granola link: https://notes.granola.ai/t/0aeed9dd-e0dd-4449-8c9c-ac79a05d817b
