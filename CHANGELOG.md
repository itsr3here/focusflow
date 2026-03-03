# Changelog

## v2.3.1 — AI Chat Robustness & Gmail Fix (2026-03-03)

### 🤖 Smarter AI Responses
- AI assistant now provides meaningful, context-aware replies instead of generic messages
- Task queries display rich task cards with priority, due date, and completion status
- Failed operations show clean, user-friendly error messages with actionable guidance
- Duplicate responses are automatically consolidated for a cleaner chat experience

### 📧 Gmail Integration Fix
- Fixed an issue where the AI assistant incorrectly stated it couldn't access emails
- Email reading, searching, sending, replying, and inbox management now work as expected
- AI can now fully manage Gmail: read, search, send, reply, star, archive, and trash emails

### 🎨 Light Mode & Theme Fixes
- Resolved visual inconsistencies in AI chat when using light mode
- Replaced hardcoded colors with theme-aware variables across the chat interface
- AI-generated options now render as interactive clickable buttons
- Resizable AI chat panel for a more flexible workspace

### 🛡️ Error Handling Improvements
- Database-related errors are now caught and presented as user-friendly messages
- All error states display with consistent formatting and helpful icons
- Empty data responses now show appropriate "no results" messages instead of blank content

---

## v2.3.0 — AI Agent Intelligence Upgrade (2026-03-03)

### 🧠 Persistent Memory System
- AI agent can now remember preferences, facts, and instructions across sessions
- New `ai_memory` database table stores memories with type categorization
- Tools: `save_memory`, `get_memories`, `search_memories`, `delete_memory`
- Memories are loaded into context on every interaction for personalized responses

### 🔗 Multi-Step Tool Chaining
- AI can now call tools, review results, then decide to call MORE tools before giving a final answer
- Up to 5 rounds of tool chaining for complex multi-step requests
- Example: "Plan my week" → fetches tasks, calendar, habits, then synthesizes a plan

### 📊 Analytics & Trends Tools
- `get_productivity_trends`: 4-week task completion rate trends
- `get_focus_trends`: 14-day daily focus session breakdown
- `get_weekly_scorecard`: This week vs last week comparison (tasks, focus, habits)

### 🕐 Time-Aware Context & Suggestions
- Context now includes day-of-week behavioral hints (Monday planning, Friday wrap-up, weekend relaxation)
- Time-of-day hints (morning deep work, afternoon progress check, evening wind-down)
- Dynamic suggestion chips that change based on time and user state
- Overdue tasks, at-risk streaks, and 48h deadlines shown proactively

### 📅 Natural Language Dates
- All date-accepting tools now understand natural language ("tomorrow", "next Friday", "in 3 days")
- Powered by chrono-node with forward-date preference
- Applied to: create_task, create_calendar_event, create_reminder

### 🔔 Proactive Notifications
- New proactive nudge service runs every 30 minutes during waking hours (7am-10pm)
- Streak risk alerts for habits with 3+ day streaks not yet done today
- Overdue task notifications
- Focus session reminders when behind daily goal
- Weekly retro prompt on Saturday evenings
- Smart deduplication prevents notification spam

### 💬 Conversation Summarization
- Each tool-using conversation is summarized and stored in `ai_conversation_summaries`
- Rolling context of last 20 conversations feeds into future interactions
- Enables continuity across sessions without full history replay

### 🎨 Enhanced Context Snapshot
- Now includes: overdue task names, at-risk habit streaks, 48h deadlines
- Persistent memories loaded into every context
- Recent conversation summary for rolling context
- Upgraded system prompt with 7 behavioral guidelines

### 🛡️ Confirmation for Destructive Actions
- System prompt instructs AI to confirm before bulk deletes and bulk changes
- Reduces risk of accidental data loss

### ⚡ Smart Suggestions
- New `ai:getSuggestions` IPC endpoint for dynamic suggestion chips
- Morning: "Plan my day", "CEO Cockpit summary"
- Afternoon: "How is my day going?", "Productivity trends"
- Evening: "Review my day", "Weekly scorecard"
- Contextual: overdue task counts, at-risk streak counts

## v2.2.6 — Llama 3.2 Integration & Ollama Support (2026-03-02)

### ✨ Features
- **Local AI (Ollama)**: Integrated a completely new, built-in model provider routing engine specifically designed for self-hosted instances. 
- **Llama 3.2 Built-In**: Configured `llama-3.2` as the new default, out-of-the-box system model. Features completely free, fully private AI execution through Oracle VMs. 
- **Tool Calling Parity**: Developed a robust format translator that converts FocusFlow's (Gemini-native) tool schemas over to Ollama's expected function arguments. Handled subsequent assistant follow-up flow perfectly.
- **AI Picker Uplift**: Upgraded the model selection UI to tag "Self-hosted" models clearly while introducing a new "Rate Limited" amber warning tooltip wrapper around the classic Gemini cloud models.
- **Dynamic Configuration**: Added full `legacy_json_data` persistence support so users can dynamically change the backing Ollama URL and default model name securely right from `window.api.ai`.

---

## v2.2.2 — Structural Fixes & GitHub Actions (2026-03-01)

### 🛠 Fixes & Tooling
- **Data Inconsistency**: Built an automatic SQL trigger system mirroring task modifications into the parent `projects` metadata, keeping task counters perfectly synced. Resolved the desynchronized test database.
- **Referential Integrity**: Implemented `PRAGMA foreign_keys = ON;` in SQLite connection setup to strictly disallow orphaned records.
- **Missing Code Tooling**: Overhauled and cleaned ESLint `v8` configurations handling `.js/.jsx/.ts/.tsx` correctly, fixing broken `lint` command pipelines.
- **React Anti-pattern**: Refactored `useHabitomicData.js` performance hooks, cleanly moving functional traps off `useMemo` strictly over to `useCallback`.
- **CI/CD Automation**: Deployed `.github/workflows/release.yml` automating multi-platform CI/CD packaging on tagged pushes.

---

## v2.2.1 — Mail Engine Enhancements & Robust AI Spec (2026-02-28)


### ✨ Features & Specifications
- **Mail Engine Spec**: Drafted an exhaustive, enterprise-grade `mail_engine_features.md` specification detailing core infrastructure, advanced AI/CRM triage, deep ecosystem hooks, and security.
- **AI Agent Spec**: Designed a bleeding-edge, hyper-futuristic `ai_agent_spec.md` with capabilities such as ambient context awareness, predictive pre-computation, agent-to-agent negotiation, physical IoT syncing, and biometric data logging.

### 🐛 Bug Fixes
- **Paywall Caching Fixed**: Resolved an issue where the free-tier entitlement cache persisted during a new Google login. The app now properly invalidates the local device cache and enforces a clean backend check when a new user signs in, guaranteeing smooth access to Pro features.

---

## v2.2.0 — Security Hardening & Performance (2026-02-27)

### 🔒 Security

- **Overlay windows**: Fixed critical vulnerability — replaced `nodeIntegration: true` with `contextIsolation: true` + dedicated preload script (`overlay-preload.ts`)
- **CSP hardened**: Removed `unsafe-eval` and `file:` from Content Security Policy
- **Secrets externalized**: APP_KEY no longer read from `.env` — hardcoded as client identifier (not a real secret). CACHE_SECRET auto-generated per device
- **macOS**: Enabled `hardenedRuntime` for notarization

### ⚡ Performance

- **Code splitting**: All 15 page components now lazy-loaded via `React.lazy()` + `Suspense`
- **Memoization**: Added `React.memo()` to 8 largest components (Habitomic, Settings, Focus, Tasks, BookShelf, Mail, Healthify, SpotifyPlayer)
- **Async I/O**: Local HTTP server now uses `fs.promises` instead of blocking `readFileSync`

### 🛠 Maintenance

- **React StrictMode**: Enabled for development bug detection
- **TypeScript strict mode**: Enabled `strict: true` + `noImplicitAny` in Electron config
- **ESLint**: Now lints `.js/.jsx` files (was only `.ts/.tsx`)
- **Testing**: Added Vitest + jsdom with initial test suite (`npm test`)
- **CI/CD**: Added GitHub Actions workflow (lint → test → build on Ubuntu + Windows)

### 📦 Dependencies

- Replaced unmaintained `react-quill` with `react-quill-new` (maintained fork)
- Removed unused `youtube-ext` and `zustand`
- Fixed `react-is` version (v19 → v18 to match React 18)
- Corrected runtime vs dev dependency placement (`dnd-kit`, `framer-motion`, `lucide-react`, etc.)

### 🐛 Bug Fixes

- Fixed duplicate `ErrorBoundary.jsx` / `.tsx` conflict
- Fixed named vs default import for `ErrorBoundary`
- Fixed backend `db.json` write crash on Vercel (EROFS: read-only filesystem)

---

## v2.1.1

- Auto-updater download chain and UI persistence fixes
