# Stratis — Feature Specification
> Strategic Decision Intelligence · v3 Prototype Audit

---

## App Overview

**Stratis** is a Strategic Decision Intelligence tool. It records meetings via a hardware device ("Pebble"), auto-captures signals from connected tools (Slack, Notion, Jira, Google Calendar), and maps them onto a living strategy canvas so leadership teams can track decisions, risks, and assumptions in real time.

---

## 1. Icon Rail
*Left sidebar — always visible across all panels*

| Icon | Tooltip | Function |
|---|---|---|
| `ti-git-branch` (logo) | — | App logo / branding. No nav in prototype. Production: click → home / projects dashboard |
| `ti-layout-grid` | All projects | Navigate to **Projects panel** |
| `ti-git-branch` | Strategy map | Navigate to **Strategy Map panel** for current project |
| `ti-microphone` + pulsing red badge | In meeting now | Navigate to **Meeting panel** — live transcript & signal capture. Badge pulses while recording is active |
| `ti-check-square` + red badge | Review decisions | Navigate to **Decisions panel** — open, blocked, and resolved decisions. Badge = unread count |
| `ti-inbox` + red badge | Signals inbox | Navigate to **Inbox panel** — aggregated signals from all connected sources. Badge = unread count |
| `ti-settings` | Settings & integrations | Navigate to **Integrations panel** |
| User avatar `SK` | Sarah K. | Opens user profile / account settings *(not implemented in prototype)* |

---

## 2. Context Drawer
*Second sidebar — project-scoped. Opens for Map, Meeting, Decisions, Inbox. Collapses for Projects and Settings.*

| Element | Function |
|---|---|
| Project name "Pricing v2" | Displays current project. Should link to project settings on click. |
| **Strategy map** | Switch to Map panel; marks itself active |
| **In this meeting** + "Live" badge | Switch to Meeting panel. Badge = recording is active |
| **Reviewing decisions** + number badge | Switch to Decisions panel. Badge = open decisions needing input |
| **Document** | Switch to Document panel (living strategy brief) |
| **Checking signals** + number badge | Switch to Inbox panel. Badge = unread signal count |
| **Mobile launch / Enterprise GTM / Q3 OKRs** | Switch context to another project *(not fully wired in prototype)* |
| **Pebble device pill** (bottom) | Tap to toggle recording on/off — pauses or resumes live transcript capture. Shows animated recording dot + elapsed timer |

---

## 3. Panel: All Projects

| Element | Function |
|---|---|
| `ti-filter` Filter | Filter and sort project grid (by status, priority, owner, etc.) |
| `ti-plus` New project *(primary)* | Create a new strategic project — opens creation modal |
| Project cards (Pricing v2, Mobile launch, Enterprise GTM) | Click → navigate into that project's Strategy Map panel |
| Recently viewed rows | Shortcuts back to the exact panel last visited for that item |

---

## 4. Panel: Strategy Map

### 4.1 Topbar

| Element | Function |
|---|---|
| Breadcrumb "Pricing v2 › Strategy map" | Navigation context. Clicking the project name should navigate to project settings |
| `ti-filter` Filter | Filter nodes on canvas by type, owner, or status |
| `ti-player-play` Replay | Animate the timeline from the beginning — shows how decisions evolved over time. Click again to stop |
| `ti-plus` Add node *(primary)* | Open a form to add a new node (decision, assumption, risk, option) to the map |
| Changes banner `ti-x` dismiss | Dismisses the changelog/update banner |

### 4.2 Map Toolbar

| Element | Function |
|---|---|
| **All types** *(active by default)* | Show all node types on canvas |
| **Decisions** | Filter canvas to decision nodes only |
| **Risks only** | Filter canvas to risk nodes only |
| **Assumptions** | Filter canvas to assumption nodes only |
| **Exec** zoom | Executive view: collapses option clusters, hides non-critical nodes, shows a callout summary — designed for leadership review |
| **Standard** zoom *(default)* | Full node view |
| **Deep** zoom | Full detail view: shows evidence bars and all metadata on every node |

### 4.3 Canvas Nodes
*All nodes are clickable. Clicking a node loads its detail into the right-hand Detail Panel.*

| Node | Type | Visual State |
|---|---|---|
| Q2 revenue miss | Origin event | Green border, root cause styling |
| Restructure pricing tiers | Decision | Amber pulsing glow (hot-debate), 14d age indicator |
| Option A: Seat-based + overages | Option | Neutral / Safe tag |
| Option B: Pure usage-based | Option | Blue pulsing glow (AI-recommended), green border (validated) |
| Option C: Value-based pricing | Option | High effort tag |
| SMB accepts metered billing | Assumption | Amber border, Unverified, 94d freshness decay |
| Engineering ships in 6 weeks | Assumption | Red border, False status, blocked indicator, 8d age |

### 4.4 Cluster Controls

| Element | Function |
|---|---|
| **Collapse / Expand** on Options cluster | Collapses the three option nodes into a summary pill or expands them |
| **Collapse / Expand** on Assumptions cluster | Collapses or expands the two assumption nodes |

### 4.5 Detail Panel *(right side of map)*

| Element | Function |
|---|---|
| **Detail** tab | Shows context text, confidence bars (AI / stakeholder / evidence), AI-generated clarifying questions, source badges, and strategic phase tracker (Explore → Align → Validate → Commit → Rollout) |
| **Context** tab | Shows node relationship graph (depends on / blocks / blocked by) and team dynamics — each member's stance (supports / blocks / concern) |
| **Risks** tab | Lists risks flagged for the selected node with severity indicators (red / amber / grey) |
| **Live** tab *(red dot)* | Shows meeting transcript lines that have been pinned to this specific decision |
| AI input bar — "Ask about this decision…" | Free-text field + **Ask** button. Sends query to Claude API scoped to the selected node. Returns contextual analysis inline |

### 4.6 Transcript Drawer
*Collapsible bottom bar on the Map panel*

| Element | Function |
|---|---|
| Header bar | Click to expand/collapse the live transcript feed |
| `ti-map-pin` Pin icon *(per line)* | Pins that spoken statement to the strategy map as a signal on the relevant node |
| Signal counter | Auto-increments as new transcript lines stream in |

### 4.7 Decision Velocity Bar
*Fixed bottom bar — read-only metrics*

| Metric | Description |
|---|---|
| **Velocity** | Whether the decision is moving (Active) or stuck (Stalled) |
| **Debate loops** | How many times an option has been reopened |
| **Blockers** | Count of active blocking issues |
| **AI prediction strip** | Estimated additional delay if current velocity continues — generated by Claude API |

### 4.8 Strategic Timeline
*Scrubable bar at the bottom of the map canvas*

| Element | Function |
|---|---|
| Timeline track | Drag to scrub through time — nodes appear/disappear to reflect the map state at that date |
| Event dots | Hover for tooltip with event name. Click should freeze the map at that point in time |
| Replay button *(also in topbar)* | Auto-animates the timeline from start to present |

---

## 5. Panel: In This Meeting

| Element | Function |
|---|---|
| **Open strategy map** button | Switches to Map panel while keeping meeting context |
| **End & summarise** button *(red, primary)* | Ends the recording session. Triggers AI-generated meeting summary with decisions captured, risks flagged, and action items |
| Participant avatars | Shows meeting attendees. Active speaker avatar pulses |
| **Pin to map** *(per transcript block)* | Pins that speaker's statement to the relevant node on the strategy map |
| **Mark decision** *(per transcript block)* | Flags the statement as a formal decision point and adds it to the Decisions panel |
| **Flag risk** *(per transcript block)* | Creates a risk entry linked to the current node |
| **Add option** *(per transcript block)* | Creates a new option node on the strategy map from within the meeting |
| Sidebar — captured items **View** | Opens the related node in the strategy map |
| Sidebar — captured items **Pin** | Anchors the captured signal to the map node |

---

## 6. Panel: Review Decisions

| Element | Function |
|---|---|
| `ti-plus` Add decision *(primary)* | Manually log a new decision |
| **All / Needs input / Blocked / Resolved** filter chips | Filter the decision list |
| Decision cards (D-13, D-14, D-16) | Expandable rows showing context, owner, due date, and map link |
| Option buttons on D-14 ("30 days" / "12 months") | Quick-select between options — records the team's choice |
| Option buttons on D-13 ("20 accounts" / "50 accounts" / "Full cohort") | Quick-select pilot scope |
| `ti-git-branch` On strategy map *(in card footer)* | Navigates to Map panel with that node selected |

---

## 7. Panel: Signals Inbox

| Element | Function |
|---|---|
| `ti-check` Mark all read | Clears all unread indicators |
| Inbox cards | Click → opens related node or source document |
| Source icons | Visual indicator of signal origin (Slack, Notion, AI, Risk) |
| Tags (Risk / Signal / Historical / Doc) | Signal classification — should be filterable in production |

---

## 8. Panel: Document

| Element | Function |
|---|---|
| **Open in map** button | Switches to Map panel with the related node selected |
| `ti-download` Export *(primary)* | Exports document as PDF or shareable link |
| TOC items (Overview, Option comparison, Open decisions, Assumptions, Risk log) | Jump-to anchors within the document body |
| TOC — Related nodes (D-14, D-16, Assumption nodes) | Click → switches to Map panel with that node selected |
| TOC — History (v1, v2, v3) | Opens a previous version of the document |
| Decision reference rows (#D-14, #D-15, #D-16) | Click → switches to Decisions panel |
| Aside — Node connection mini-cards | Click → switches to Map panel with that node selected |
| Aside — **Summarise open decisions** | Pre-built AI query → Claude API call, returns summary inline |
| Aside — **Compare all options** | Pre-built AI query → Claude API call, returns comparison |
| Aside — **Show risk analysis** | Pre-built AI query → Claude API call, returns risk breakdown |

---

## 9. Panel: Settings & Integrations

| Element | Function |
|---|---|
| Integration cards — Connected (Slack, Notion, Google Cal) | Show connection status, last sync time. Click → manage or disconnect |
| Integration card — Not connected (Jira) | Click → open OAuth flow to connect |
| Stratis Pebble device card | Shows: device ID, firmware version, battery level, recording status. Should include: pair new device, firmware update, forget device actions |

---

## 10. Full Feature List

### Core
1. Multi-project workspace with priority tagging (High / Medium / Planning)
2. Visual strategy map with typed nodes: Origin, Decision, Option, Assumption, Risk
3. Cluster grouping with collapse/expand for Options and Assumptions
4. Bidirectional links between nodes (depends on / blocks / blocked by)
5. Zoom levels: Executive, Standard, Deep — controls cognitive load
6. Node filtering by type (Decisions, Risks, Assumptions, All)

### Decision Intelligence
7. Decision aging — nodes glow red when unresolved past a threshold
8. Decision velocity metrics: stall days, debate loop count, blocker count
9. AI delay prediction based on current debate velocity
10. Stakeholder stance tracking per decision (supports / blocks / concern)
11. Confidence scoring: AI confidence, stakeholder alignment, evidence quality
12. Strategic phase tracker: Explore → Align → Validate → Commit → Rollout

### Assumptions & Risks
13. Assumption confidence bars (evidence quality score)
14. Context decay bars — freshness indicator based on how old the data is
15. Auto-flagging of assumptions as False when contradicted by meeting speech or signals
16. Risk log with severity (High / Medium / Low) and active/monitoring status

### Meeting Mode
17. Live meeting transcript with real-time speaker diarization
18. Signal auto-classification from speech: decision, risk, assumption tags
19. "Pin to map" — link any spoken statement to a map node
20. "Mark decision", "Flag risk", "Add option" inline actions per transcript block
21. AI insight cards auto-inserted when speech contradicts a map assumption
22. Meeting sidebar: real-time capture log with View / Pin actions
23. "End & summarise" — AI-generated meeting debrief with decisions, risks, and actions

### Signals & Integrations
24. Signals inbox aggregating Slack, Notion, Jira, Calendar, Email
25. Signal classification: Risk / Signal / Historical / Doc tags
26. Unread tracking and "Mark all read"
27. Live transcript drawer on the map panel (expandable)

### Document
28. Living document linked bidirectionally to strategy map nodes
29. Document versioning (v1, v2, v3) with history navigation
30. Inline decision reference rows with status badges
31. Option comparison table (auto-populated from map)
32. Assumptions and Risk log sections auto-synced from map state

### AI
33. Per-node contextual Q&A via the AI ask bar
34. AI-generated clarifying questions per decision node
35. Pre-built AI shortcuts: summarise decisions, compare options, risk analysis
36. Pattern detection: historical signals from similar industry events
37. AI recommendation badge on the highest-confidence option

---

## 11. APIs & Tools Required

### AI / LLM

| Feature | API |
|---|---|
| Contextual Q&A per node (ask bar) | **Anthropic Claude API** — `/v1/messages`, claude-sonnet-4-20250514 |
| Signal classification from transcript (decision / risk / assumption) | Claude API with structured JSON output |
| AI confidence scoring for nodes | Claude API, periodic re-scoring as signals arrive |
| Decision velocity delay prediction | Claude API |
| Meeting summary on "End & summarise" | Claude API |
| Historical pattern detection ("Intercom 2022" style signals) | Claude API + web search tool |
| AI clarifying questions per node | Claude API |
| Aside pre-built AI query shortcuts | Claude API |

### Real-time Audio & Transcription

| Feature | Tool |
|---|---|
| Live meeting transcription from Pebble device | **Deepgram** or **AssemblyAI** (streaming STT) |
| Speaker diarization (who said what) | AssemblyAI speaker detection or Deepgram diarize |
| Audio streaming from Pebble hardware | Bluetooth → WebSocket audio stream to backend |
| Fallback: cloud meeting transcription | **OpenAI Whisper** for async processing |

### Integrations (OAuth)

| Integration | API |
|---|---|
| Slack — signal capture from channels | Slack Events API + Bot token (OAuth 2.0) |
| Notion — document sync | Notion API |
| Google Calendar — meeting detection and participant sync | Google Calendar API (OAuth 2.0) |
| Jira — issue tracking (PRICE-24, PRICE-25 references) | Jira REST API (Atlassian OAuth) |
| Email signals | Gmail API or IMAP/SMTP |

### Backend & Infrastructure

| Need | Tool |
|---|---|
| Real-time transcript streaming to UI | WebSockets (Socket.io or native WS) |
| Node/edge graph storage for strategy map | Graph DB: **Neo4j** or Postgres with JSONB |
| Document versioning (v1 / v2 / v3) | Backend version store, e.g. append-only event log |
| User auth and team/workspace management | Auth0, Clerk, or Supabase Auth |
| PDF export | Puppeteer or a headless Chrome renderer |
| Context decay and aging calculations | Cron-based backend jobs computing staleness on a schedule |
| Decision velocity time-series tracking | Time-series log of state changes per decision node |
| AI confidence re-scoring on new signals | Event-driven Claude API call triggered on signal ingestion |

---

*Last updated: Prototype v3 audit — all items reflect the current HTML prototype state.*
