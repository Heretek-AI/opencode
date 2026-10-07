# Track 2 — OpenCode Core Friction & Deficit Analysis

Date of research: 2026-10-07. Researcher: CommunityTrack (Heretek-AI "Better Mouse Trap" fact-finding).

---

## 0. Upstream verification

- **The real upstream is `anomalyco/opencode`.** `repo_view` on `sst/opencode` resolves/redirects to `anomalyco/opencode` ("The open source coding agent.", https://github.com/anomalyco/opencode) — the `sst/opencode` name now maps to the anomalyco org. Stats from `repo_view` (2026-10-07): **212,153 stars, 28,254 forks, default branch `dev`, not archived, last updated 2026-10-07**. The brief's claim `anomalyco/opencode` is therefore correct.
- **The local checkout is a fork.** Local git remote: `origin https://github.com/Heretek-AI/opencode` (branch `v2`; head commit `119cb15d1d "refactor(app): remove configurable default server (#53731)"`). `repo_view` on `Heretek-AI/opencode`: `Fork: true`, viewer permission ADMIN, 0 stars. Issue/PR numbering has passed **53k** (e.g. PR #53704 created 2026-10-07, issue #53611 created 2026-10-06), confirming the volume claim in the brief.

## 1. Method + sample disclosure

**Tooling.** GitHub data obtained via the `xd://github` device (`repo_view`, `search_issues`, `search_prs`, GitHub search syntax scoped to `anomalyco/opencode`) and full-text spot reads via `issue://anomalyco/opencode/<N>` and `pr://anomalyco/opencode/<N>`. All searches executed 2026-10-07.

**Queries run (query → results returned, capped per query):**

| # | Query | Type | Returned |
|---|-------|------|----------|
| 1 | `is:issue is:open sort:reactions` | issues | 20 |
| 2 | `is:issue session state database corruption sqlite` | issues | 20 |
| 3 | `is:issue compaction context window loses instructions` | issues | 13 |
| 4 | `is:issue subagent timeout communication` | issues | 20 |
| 5 | `is:issue TUI freeze resize rendering` | issues | 6 |
| 6 | `is:issue sandbox permission dangerous command docker` | issues | 3 |
| 7 | `is:issue rate limit backoff retry provider stream disconnect` | issues | 5 |
| 8 | `is:issue permission prompt approval security` | issues | 20 |
| 9 | `is:issue is:open subagent` | issues | 25 |
| 10 | `is:issue infinite loop tool call token` | issues | 20 |
| 11 | `is:issue sandbox container e2b firecracker isolation` | issues | **0** |
| 12 | `is:issue vim keybind startup latency slow` | issues | **0** |
| 13 | `is:issue vim` | issues | 15 |
| 14 | `is:issue startup slow latency` | issues | 15 |
| 15 | `is:issue session fork` | issues | 15 |
| 16 | `is:issue compaction memory hierarchy context` | issues | 5 |
| 17 | `is:issue 429 rate limit` | issues | 15 |
| 18 | `is:issue escape bypass permission shell` | issues | 12 |
| 19 | `is:pr is:open sort:reactions` | PRs | 25 |
| 20 | `is:pr is:open created:<2026-07-01 sort:reactions` | PRs | 25 |
| 21 | `is:pr is:open sandbox` | PRs | 15 |
| 22 | `is:pr is:open compaction memory` | PRs | 10 |
| 23 | `is:pr 39473` (verify referenced fix PRs) | PRs | 5 |

**Date windows.** No explicit date filter except query 20 (`created:<2026-07-01`); the sample spans 2025-06 (issue #239) through 2026-10-07 (PR #53704).

**Spot-checked via full `issue://`/`pr://` reads (existence + content verified):** issues #14970, #12711, #48827, #42264, #39931, #37852, #11012; PRs #12730, #5245, #4865, #26292, #5422. PR existence additionally verified by search listing: #39473, #43881, #44611, #42566, #40032, #12731/#12732 (cited inside #12711/#12730 bodies), #46109 (cited inside #4865 comment thread).

**Coverage limits (honest disclosure).**
- **Reaction counts are not exposed** by the `xd://github` search or issue/PR readers. The `sort:reactions` ordering is GitHub's, so top-of-list items are high-reaction items, but the numeric counts are `NOT AVAILABLE` via tooling. **Comment counts** were captured only for the 12 full-read spot checks (noted per item below).
- Each search returns only the first 15–25 hits in GitHub relevance order; there is no pagination, so clusters are representative, not exhaustive. Keyword queries are English-biased (e.g. issue #42288 is titled in Chinese and surfaced only via query 8).
- Two queries returned zero (11, 12); narrower follow-ups (13, 14, 18) were run instead. In particular, **"E2B", "Firecracker", and generic container-sandbox feature demand was NOT FOUND** via query 11 (see cluster e).
- Maintainer status of individual commenters is verified only where the comment content itself demonstrates merge authority (e.g. @rekram1-node "I will try to get this in", @thdxr refactoring announcements); otherwise labeled `UNVERIFIED`.
- Issue/PR states are as observed 2026-10-07; the repo is very high-velocity and states change.

---

## 2. Issue clusters

Each cluster lists ≥3 real issues (number — title — URL — state — created — comments where captured). "Comments (N)" values are from full reads; `n/c` = comment count not captured.

### (a) State & session persistence (SQLite corruption, session recovery, fork/branch divergence)

| # | Title | State | Created | Notes |
|---|-------|-------|---------|-------|
| [#14970](https://github.com/anomalyco/opencode/issues/14970) | SQLite database corruption (database disk image is malformed) when running concurrent sessions on NFS | open | 2026-02-24 | **Comments (13)**. Verified full read. Single global `~/.local/share/opencode/opencode.db`; WAL+SHM over NFS corrupts; users lose all session history; workarounds (`.recover`, per-process `XDG_DATA_HOME`) circulate in-thread; commenters explicitly ask for `OPENCODE_DATA_DIR`. Cross-version schema drift on the shared DB also bricks startup (comment 2026-07-10). Still recurring 2026-09-21 (WAL forensics comment). |
| [#53146](https://github.com/anomalyco/opencode/issues/53146) | session: two server processes sharing one opencode.db allocate session_message.seq independently — UNIQUE(seq) collisions fail sessions | open | 2026-10-04 | n/c |
| [#49367](https://github.com/anomalyco/opencode/issues/49367) | Cross-process session concurrency: two processes can run loops on the same session simultaneously | open | 2026-09-16 | n/c |
| [#31307](https://github.com/anomalyco/opencode/issues/31307) | Multiple opencode instances in the same project share the same session via SQLite database | open | 2026-06-08 | n/c |
| [#48416](https://github.com/anomalyco/opencode/issues/48416) | Daily "Failed to execute statement" on macOS with concurrent opencode processes (1.18.30, busy_timeout=0, 21 GB DB) | open | 2026-09-11 | n/c |
| [#43620](https://github.com/anomalyco/opencode/issues/43620) | OpenCode fails when two hosts share an NFS home directory | open | 2026-08-20 | n/c |
| [#49221](https://github.com/anomalyco/opencode/issues/49221) | Startup permanently OOMs and bricks the app if any session's data fails schema validation (no graceful recovery) | open | 2026-09-15 | n/c |
| [#43277](https://github.com/anomalyco/opencode/issues/43277) | Sessions permanently stuck during normal use — survive reboots, cannot be recovered | open | 2026-08-18 | n/c |
| [#37821](https://github.com/anomalyco/opencode/issues/37821) | opencode crashes on startup when SQLite database is corrupted | closed | 2026-07-19 | closed 2026-10-03 |

**Fork/branch divergence sub-cluster:** [#42341](https://github.com/anomalyco/opencode/issues/42341) "Forked parent session opens original child session, so Back returns to original parent" (open, 2026-08-13); [#39656](https://github.com/anomalyco/opencode/issues/39656) "V2 session: auto-revert leaves forked session without instruction checkpoint" (closed, 2026-07-30, label `2.0`); [#48306](https://github.com/anomalyco/opencode/issues/48306) "opencode2 stats under-reports spend when a forked session's parent is deleted" (open, 2026-09-10, label `2.0`); [#45242](https://github.com/anomalyco/opencode/issues/45242) "app: /fork dialog only lists currently loaded messages" (open, 2026-08-26); [#41698](https://github.com/anomalyco/opencode/issues/41698) "[BUG]: Forking long sessions takes several seconds" (open, 2026-08-11); [#40169](https://github.com/anomalyco/opencode/issues/40169) "session/fork and GET /session/:id/message fail for sessions containing a message with an output format" (open, 2026-08-02). V1/V2 coexistence: [#42260](https://github.com/anomalyco/opencode/issues/42260) "opencode2 mutates shared V1 database and breaks opencode 1.x coexistence" (closed, 2026-08-13, label `2.0`); [#52226](https://github.com/anomalyco/opencode/issues/52226) "V1→V2 migration silently drops v1.0.x-era assistant messages… 6,238 messages across 92 sessions" (closed, 2026-09-30).

### (b) Context window & compaction (over-compaction, infinite loops, token starvation, memory)

| # | Title | State | Created | Notes |
|---|-------|-------|---------|-------|
| [#2945](https://github.com/anomalyco/opencode/issues/2945) | Session automatically compacted, destroying the entire working context | closed | 2025-10-03 | canonical over-compaction report |
| [#15533](https://github.com/anomalyco/opencode/issues/15533) | Auto-compaction infinite loop when assistant ended its turn (finish !== tool-calls) | open | 2026-03-01 | referenced as same-root-cause by #48827 bot |
| [#27924](https://github.com/anomalyco/opencode/issues/27924) | bug(session): infinite compaction loop when compression fails to reduce context | open | 2026-05-16 | n/c |
| [#48827](https://github.com/anomalyco/opencode/issues/48827) | Auto-compaction loops forever when a session's irreducible context exceeds the model's usable window | open | 2026-09-13 | **Comments (1)**. Verified full read. `session/overflow.ts` math (`usable = context - min(limit.output, OUTPUT_TOKEN_MAX)`); loop compacts→continues→overflows forever, executes tool calls unattended, burns quota until rate-limited. Author ships a proposed no-progress guard. |
| [#49965](https://github.com/anomalyco/opencode/issues/49965) | Auto-compaction fires after every tool-call step for Ollama (openai-compatible) provider, even when nowhere near context limit | open | 2026-09-19 | n/c |
| [#36682](https://github.com/anomalyco/opencode/issues/36682) | [SECURITY]: Compaction summary injects actionable instructions — model executes without user consent | open | 2026-07-13 | compaction as prompt-injection vector |
| [#31764](https://github.com/anomalyco/opencode/issues/31764) | [BUG] compaction causes AGENTS.md to be re-attached because extract() skips compacted Read results | closed | 2026-06-10 | instruction loss/re-attachment |
| [#6535](https://github.com/anomalyco/opencode/issues/6535) | Auto-compaction causes the subagent to lose its original context, possibly the system prompt as well | closed | 2025-12-31 | labels `bug, windows` |
| [#16512](https://github.com/anomalyco/opencode/issues/16512) | [FEATURE]: Improve compaction prompt to preserve critical context details | closed | 2026-03-07 | n/c |

**Infinite tool loops / token starvation sub-cluster:** [#45442](https://github.com/anomalyco/opencode/issues/45442) "subagent: infinite loop of identical tool calls for ~50min with no loop protection, uncontrollable token burn" (open, 2026-08-27, label `2.0`); [#49042](https://github.com/anomalyco/opencode/issues/49042) "Agent loop runs 500+ steps with zero user input — no guard against runaway auto-continue" (open, 2026-09-14); [#47253](https://github.com/anomalyco/opencode/issues/47253) "DeepSeek-V4-Flash-0731 consumed ~22M tokens in prolonged agent loop…" (open, 2026-09-04); [#18108](https://github.com/anomalyco/opencode/issues/18108) "Truncated tool calls are misclassified and unrecoverable (finishReason: length + repairToolCall + doom loop)" (open, 2026-03-18); [#25129](https://github.com/anomalyco/opencode/issues/25129) "Thinking mode gets stuck in infinite repetition loop (!!!!!!!!!!)" (open, 2026-04-30).

**Memory/hierarchy sub-cluster:** [#20695](https://github.com/anomalyco/opencode/issues/20695) "Memory Megathread" — opened by maintainer @thdxr (closed 2026-09-30; canonical memory discussion hub); [#30116](https://github.com/anomalyco/opencode/issues/30116) "[FEATURE]: Memory compaction awareness hooks for agents" (closed, 2026-05-31); [#19471](https://github.com/anomalyco/opencode/issues/19471) "[FEATURE]: Expose context budget information to agents" (closed, 2026-03-27); [#6152](https://github.com/anomalyco/opencode/issues/6152) "[FEATURE]: Session context usage (similar to /context in Claude)" (open, 2025-12-25, top-reaction list); [#41354](https://github.com/anomalyco/opencode/issues/41354) "[FEATURE]: Search across my message history…" (open, 2026-08-09). No dedicated "hierarchical memory" implementation issue was found; the closest are #20695/#30116 (`NOT FOUND` as a standalone issue: searched queries 3 and 16).

### (c) Multi-agent & subagent deficits (comms, long-running vs parent timeouts, swarm/consensus)

| # | Title | State | Created | Notes |
|---|-------|-------|---------|-------|
| [#11012](https://github.com/anomalyco/opencode/issues/11012) | SubAgents are enclosed, prohibiting genuine task management with OpenCode. | closed **NOT_PLANNED** | 2026-01-28 | **Comments (6)**. Verified full read. Subagent sessions can't be re-entered/directed after return. Bot comment enumerates 14 sibling issues (#6584, #6491, #6907, #5887, #7296, #9280, #2906, #3023, #6299, #2378, #6792, #10374, #4422, #10802). Commenter: "It was really sad seeing subagents reduced to what they are now" — direct chat with subagents was removed after briefly existing. Auto-closed after 60 days inactivity (see §4). |
| [#12711](https://github.com/anomalyco/opencode/issues/12711) | [DESIGN]: Agent Teams — flat teams with named messaging, multi-model support, and TUI integration | open | 2026-02-08 | **Comments (15)**. Verified full read. Complete working design + 3-PR stack (~11.7k lines incl. tests); demand comments through 2026-09; community prototypes built externally (repowire, spark-agent-teams, AgentNexus suggestions). |
| [#50751](https://github.com/anomalyco/opencode/issues/50751) | subagent: background subagent's resumed turn completes but the parent session never receives the completion notification | open | 2026-09-22 | n/c |
| [#44931](https://github.com/anomalyco/opencode/issues/44931) | subagent: background subagents hang silently on repeated upstream 429s; no error or completion notification surfaces | open | 2026-08-25 | label `2.0`; crosses cluster f |
| [#43996](https://github.com/anomalyco/opencode/issues/43996) | Permission prompts not propagated at subagent_depth 2 — subagent hangs waiting for approval | open | 2026-08-21 | n/c |
| [#36423](https://github.com/anomalyco/opencode/issues/36423) | v2: subagent: no cancellation support for background subagents | open | 2026-07-11 | label `2.0` |
| [#36670](https://github.com/anomalyco/opencode/issues/36670) | [BUG] Subagent final part lost — SessionProcessor.cleanup races with "removing share" (1.17.18) | open | 2026-07-13 | n/c |
| [#37852](https://github.com/anomalyco/opencode/issues/37852) | Aborted provider stream recorded as clean stop (finish=unknown, zero usage, no text) — subagent returns empty with no error | open | 2026-07-20 | **Comments (19)**. Verified full read. Parent cannot distinguish provider dropout from success; "particularly dangerous for subagent workflows" (comment 2026-08-19). Crosses cluster f. |

**Comms/UX demand sub-cluster (all open):** [#52795](https://github.com/anomalyco/opencode/issues/52795) "[FEATURE]: Subagents should ask questions to the parent session, not to the user" (2026-10-02); [#52867](https://github.com/anomalyco/opencode/issues/52867) "[FEATURE]: Let users message a subagent directly from its view" (2026-10-02); [#53030](https://github.com/anomalyco/opencode/issues/53030) "[FEATURE]: Notify subagents when their context are running out" (2026-10-03); [#52016](https://github.com/anomalyco/opencode/issues/52016) "[FEATURE]: Stop a running background subagent from the web app" (2026-09-29); [#53611](https://github.com/anomalyco/opencode/issues/53611) "[FEATURE]: Show running subagents under the prompt" (2026-10-06); [#51113](https://github.com/anomalyco/opencode/issues/51113) "web: main agent does not surface running or stuck subagents" (2026-09-24); [#47995](https://github.com/anomalyco/opencode/issues/47995)/[#47996](https://github.com/anomalyco/opencode/issues/47996) permission-rejection fan-out bugs across parallel subagents (2026-09-08).

**Swarm/consensus sub-cluster:** #12711 (above); [#12661](https://github.com/anomalyco/opencode/issues/12661) "[FEATURE]: Add Agent Teams Equivalent or Better" (closed, 2026-02-08); [#20368](https://github.com/anomalyco/opencode/issues/20368) "[FEATURE]: Coordinator shell + fork mode" (closed, 2026-03-31); [#17311](https://github.com/anomalyco/opencode/issues/17311) "[FEATURE]: orchestration support" (closed, 2026-03-13); [#49842](https://github.com/anomalyco/opencode/issues/49842) "Proposal: background subagents, monitors, cron, worktree isolation" (open, 2026-09-18); A2A protocol integration requested as [#3023](https://github.com/anomalyco/opencode/issues/3023) (cited in #11012 bot list). Parallel-subagent limits also referenced via PR #7756 (tree delegation, cited in #12711 body — existence not independently verified).

### (d) TUI & dev experience (rendering, resize freezes, vim keys, startup latency, panels)

| # | Title | State | Created | Notes |
|---|-------|-------|---------|-------|
| [#42264](https://github.com/anomalyco/opencode/issues/42264) | TUI: text disappears during LLM streaming, main thread stuck in timerfd busy-loop (TreeSitter worker stack overflow) | open | 2026-08-13 | **Comments (5)**. Verified full read. Root-caused to `@opentui/core` TreeSitter WASM stack overflow + worker crash loop (5,961–16,378 SIGSEGVs per incident); text freeze is content-deterministic; still present in v1.18.31 on Linux/Windows/macOS; cited sibling issues #15388 (closed not-planned), #3905, #3935, #23914, #17793. |
| [#53165](https://github.com/anomalyco/opencode/issues/53165) | OpenCode 2.x TUI becomes partially unresponsive while agent continues running | open | 2026-10-04 | n/c |
| [#37803](https://github.com/anomalyco/opencode/issues/37803) | TUI screen goes completely black when agent starts working (render loop silently stalls) | closed | 2026-07-19 | n/c |
| [#3776](https://github.com/anomalyco/opencode/issues/3776) | Screen becomes completely blank | closed | 2025-11-02 | labels `bug, opentui` |
| [#48155](https://github.com/anomalyco/opencode/issues/48155) | TUI: streamed markdown list items truncated when items contain emphasis spans | open | 2026-09-09 | n/c |
| [#48633](https://github.com/anomalyco/opencode/issues/48633) | Desktop 1.18.30 (Windows 11): sidecar unresponsive / local server red after every chat, renderer ResizeObserver error storm | open | 2026-09-12 | resize-related freeze |
| [#43355](https://github.com/anomalyco/opencode/issues/43355) | [Desktop] UI freezes after agent turns finish — renderer stuck in ResizeObserver loop; only force-quit recovers | closed | 2026-08-19 | n/c |
| [#41477](https://github.com/anomalyco/opencode/issues/41477) | Slow startup / long loading time every time the desktop app is launched (30+ seconds) | open | 2026-08-10 | n/c |
| [#48051](https://github.com/anomalyco/opencode/issues/48051) | [macOS] attach takes seconds to connect and render first TUI frame after serve is ready | open | 2026-09-09 | n/c |
| [#37794](https://github.com/anomalyco/opencode/issues/37794) | core: cold VCS location acquisition blocked the service for 53 seconds | open | 2026-07-19 | labels `bug, perf, core, 2.0` |
| [#42376](https://github.com/anomalyco/opencode/issues/42376) | Startup takes 10-30s, blocking network fetch of models.dev/api.json on every boot when cache is stale | closed | 2026-08-13 | n/c |

**Keybindings/panels sub-cluster:** [#4714](https://github.com/anomalyco/opencode/issues/4714) "[FEATURE]: TUI - Search for and find string in session buffer" (open since 2025-11-24, still active 2026-10-07); [#4997](https://github.com/anomalyco/opencode/issues/4997) "Keybinds" (open since 2025-12-02, top-reaction list); [#1764](https://github.com/anomalyco/opencode/issues/1764) "[FEATURE]: vim motions in input box" (closed, 2025-08-09) and [#11111](https://github.com/anomalyco/opencode/issues/11111) "[FEATURE]: VIM Keyboard Layout" (closed, 2026-01-29) — both closed despite demand; [#43267](https://github.com/anomalyco/opencode/issues/43267) "diff: add configurable half-page scrolling keybindings" (open, 2026-08-18, label `2.0`); [#51296](https://github.com/anomalyco/opencode/issues/51296) "[FEATURE]: Bindable redraw/repaint command (e.g. Ctrl+L) for TUI" (open, 2026-09-25); [#1168](https://github.com/anomalyco/opencode/issues/1168) "Feature Request: Make Links Clickable" (open since 2025-07-20, top-reaction list). Split-view/panel demand is embodied in PRs #4865 (subagent sidebar) and #21002 (`/btw` side-question panel, open since 2026-04-04) rather than standalone issues.

### (e) Tooling & sandboxing (dangerous commands, permission prompts, containerized isolation)

| # | Title | State | Created | Notes |
|---|-------|-------|---------|-------|
| [#2242](https://github.com/anomalyco/opencode/issues/2242) | Is there a way to sandbox the agent? | open | 2025-08-25 | top-reaction list; open 13+ months — the standing sandbox demand thread |
| [#39931](https://github.com/anomalyco/opencode/issues/39931) | bash permission escape via `--` double hyphen | open | 2026-07-31 | **Comments (3)**. Verified full read. `git diff --` executes with zero permission evaluation; commenter: "the bash allowlist is advisory only, not a security boundary" (verified on 1.18.15). Root cause tracked in #31485 (recurring string-parsing permission bypasses) and #39001; fixed downstream in community fork "ranex-trim" (commit `2d0950f6e5`, per comment 2026-08-13) — not merged upstream. |
| [#42979](https://github.com/anomalyco/opencode/issues/42979) | Shell redirection bypasses the external_directory permission | open | 2026-08-17 | n/c |
| [#46398](https://github.com/anomalyco/opencode/issues/46398) | Security: external_directory does not gate PowerShell/bash file writes on Windows | open | 2026-08-31 | n/c |
| [#49347](https://github.com/anomalyco/opencode/issues/49347) | Windows: `permission.bash` deny rules are skipped and built-in `bash` executes denied commands | closed | 2026-09-16 | n/c |
| [#43336](https://github.com/anomalyco/opencode/issues/43336) | A symlink inside the workspace bypasses the external_directory permission | closed | 2026-08-19 | n/c |
| [#38526](https://github.com/anomalyco/opencode/issues/38526) | [FEATURE]: Improve shell command permission request UX | open | 2026-07-23 | n/c |
| [#51859](https://github.com/anomalyco/opencode/issues/51859) | [FEATURE]: scoped auto-approval mode with expiry, persistent risk indicator and audit trail | open | 2026-09-28 | n/c |
| [#39628](https://github.com/anomalyco/opencode/issues/39628) | [FEATURE]: Remote approval of permission requests from mobile / second device | open | 2026-07-30 | n/c |
| [#10238](https://github.com/anomalyco/opencode/issues/10238) | [FEATURE]: Safer default for build agent: prompt on destructive bash commands | closed | 2026-01-23 | label `discussion` |
| [#2206](https://github.com/anomalyco/opencode/issues/2206) | [FEATURE] add `exec` tool as more secure alternative to bash tool | closed | 2025-08-23 | n/c |

**Containerized isolation (Docker/E2B/Firecracker): `NOT FOUND` as a feature-request cluster.** Query `is:issue sandbox container e2b firecracker isolation` returned 0 results (2026-10-07). The only isolation-adjacent hits are #2242 (generic sandboxing question, whose thread references running opencode *inside* containers as a workaround) and PR #50020 "fix(opencode): handle stale and inaccessible project worktrees and sandboxes gracefully" (open, 2026-09-19 — implies a "sandbox" concept exists in v2 worktree handling, details not examined). Community workarounds described in #14970 comments (docker/Colima/devcontainers) hit the SQLite-sharing bug instead. Conclusion: demand is expressed as *permission-model hardening* (bypasses above) rather than named container-sandbox integrations.

### (f) Provider & streaming stability (rate-limit backoff, SSE disconnects, multi-provider failover)

| # | Title | State | Created | Notes |
|---|-------|-------|---------|-------|
| [#37852](https://github.com/anomalyco/opencode/issues/37852) | Aborted provider stream recorded as clean stop (finish=unknown, zero usage, no text) — subagent returns empty with no error | open | 2026-07-20 | **Comments (19)**. Verified full read. Reproduced across Fireworks, opencode-go/deepseek-v4-flash, LiteLLM 402 quota, custom Anthropic providers, v1.18.3→v1.18.18 and v2; maintainer @rekram1-node engaged 2026-08-06 ("Will look ty") but behavior unfixed per 2026-08-19 comment; three separate fix PRs were **auto-closed** (see §3). |
| [#38458](https://github.com/anomalyco/opencode/issues/38458) | SSE stream closes mid-turn (not persistent as documented) | open | 2026-07-23 | n/c |
| [#42950](https://github.com/anomalyco/opencode/issues/42950) | opencode/big-pickle: intermittent socket disconnects cause silent drop (no UI error) + repeated Aborted in logs (v1.18.18, Linux/WSL2) | open | 2026-08-16 | n/c |
| [#30611](https://github.com/anomalyco/opencode/issues/30611) | Sessions fail on transient network errors instead of retrying | closed | 2026-06-03 | closed 2026-10-01 |
| [#12234](https://github.com/anomalyco/opencode/issues/12234) | Infinite retry loop when StreamIdleTimeoutError occurs during tool input generation | closed | 2026-02-05 | label `perf` |
| [#49039](https://github.com/anomalyco/opencode/issues/49039) | [Feature Request / Bug] Handle Rate Limit (Quota Exceeded 429) automatically with retry delay | open | 2026-09-14 | n/c |
| [#45989](https://github.com/anomalyco/opencode/issues/45989) | bug: infinite retry loop on rate limit without logging ("Free usage exceeded, subscribe to Go") | open | 2026-08-28 | n/c |
| [#50884](https://github.com/anomalyco/opencode/issues/50884) | Vertex Anthropic HTTP 429 produces no provider event | open | 2026-09-23 | n/c |
| [#53602](https://github.com/anomalyco/opencode/issues/53602) | tui: provider errors render as unhelpful "Error: Error", hiding the real cause | open | 2026-10-06 | n/c |
| [#52860](https://github.com/anomalyco/opencode/issues/52860) | ACP: provider status and rate-limit headers of API errors are dropped | open | 2026-10-02 | n/c |

**Failover demand sub-cluster:** [#7602](https://github.com/anomalyco/opencode/issues/7602) "[FEATURE]: Native Model Fallback / Failover Support" (open since 2026-01-10, top-reaction list, targeted by PR #26292); [#42513](https://github.com/anomalyco/opencode/issues/42513) "[FEATURE]:Auto-Failover for Rate Limit Resilience" (open, 2026-08-14); [#42502](https://github.com/anomalyco/opencode/issues/42502) "[FEATURE]:Native API Key Pool & Automatic Failover to Prevent Rate Limit Task Failures" (open, 2026-08-14); [#42765](https://github.com/anomalyco/opencode/issues/42765) "[FEATURE]: Warn users before free tier quota exhaustion instead of silent 429" (open, 2026-08-15); [#47039](https://github.com/anomalyco/opencode/issues/47039) "Preserve HTTP status on UnknownError so plugins can fallback" (open, 2026-09-03). Free-tier/quota pain is loud: #47613 (Go subscription 429 outage), #43786, #42977, #50160 (all open, Aug–Sep 2026).

---

## 3. Stalled / abandoned high-value PRs

Ages computed to 2026-10-07. All five headline PRs were full-read via `pr://`.

1. **[#12730](https://github.com/anomalyco/opencode/pull/12730) — "feat: add agent team core — Team namespace, messaging, recovery, and events"** (with stack [#12731](https://github.com/anomalyco/opencode/pull/12731) tools/routes, [#12732](https://github.com/anomalyco/opencode/pull/12732) TUI). Opened 2026-02-08 by @ugoenyioha; **open ~8 months**; `Merge state: DIRTY`; 2,161+ lines core + 8.5k lines tests (per #12711). Adds: Team namespace with CRUD/persistence/recovery, name-addressed messaging with auto-wake, shared task list, plan-approval gates, delegate mode, TUI sidebar/status — behind `OPENCODE_EXPERIMENTAL_AGENT_TEAMS`.
   **Stall evidence:** 14 comments, all community: "Can we please get this merged?" (2026-03-23), "No chance to merge?" (2026-04-14), "Why hasn't this merged yet?" (2026-04-21), repeated through 2026-06-09. **Zero maintainer comments observed.** Only the repo bot nagged ("All PRs must reference an existing issue" — CONTRIBUTING "issue-first policy"). **Cause: unknown (no maintainer verdict on record).**

2. **[#5422](https://github.com/anomalyco/opencode/pull/5422) — "feat(provider): add provider-specific cache configuration system (significant token usage reduction)"**. Opened 2025-12-12 by @ormandj; **open ~10 months**; `DIRTY`; 874-line `provider/config.ts` + 215 tests; A/B with Claude Opus 4.5 shows 44% cache-write and 73% effective-cost reduction; reviewed 👍 by @Sewer56 ("Needs a second pair of eyes but 👍 from me").
   **Stall evidence (maintainer quotes):** founder @thdxr, 2025-12-12: *"i'm actually currently mid refactor on a bunch of code where we call LLMs so we probably can't explicitly accept this PR (llm-centralization branch) … also i don't know if we want to go as far as making all this deeply configurable yet … i'll do a pass once that llm-centralization branch is merged"*. That pass never came; author quit 2026-01-28: *"the maintainers made it clear when I first submitted this they had other plans … I don't think it's a priority for them, and I'm not going to invest time in reworking the patch"*. Users forked it (`johnnymo87/opencode-cached`, 2026-02-28) or rebuilt as plugins (comments 2026-04). **Cause: maintainer design preference ("not deeply configurable yet") + abandoned follow-up.**

3. **[#5245](https://github.com/anomalyco/opencode/pull/5245) — "feat: integrate OpenTelemetry"**. Opened 2025-12-08 by @tianhuil; **open ~10 months**; `DIRTY`; adds OTel deps + `tracing.ts` + span wiring in session/mcp/tool paths.
   **Stall evidence:** maintainer @rekram1-node, 2025-12-08: *"I think opencode already emits telemetry spans if you set: experimental_opentelemetry to true in your opencode.json"* — contradicted by two independent users ("doesn't work for me either", 2026-01-06) and by @strangemonad (2026-03-23: *"This isn't quite true since there's never a span collector set up"*). rekram1-node later posted env-var instructions (2026-05-27) without touching the PR; thread ends with "Any update?" (2026-07-13). Earlier OTel PR #2735 also "got very little traction" (comment 2026-01-16). **Cause: maintainer believes existing experimental path suffices; conflicting docs; PR left in limbo.**

4. **[#4865](https://github.com/anomalyco/opencode/pull/4865) — "feat: add subagents sidebar with clickable navigation and parent keybind"**. Opened 2025-11-28 by @franlol; **open ~10.5 months** (oldest in sample); shows all subagents in sidebar, click-to-navigate, `<leader>+Up` return.
   **Stall evidence:** maintainer @rekram1-node, 2025-12-02: *"Yeah our designer had some opinions on this, I will try to get this in once we have UX thought out, prolly will update this directly"* — no further maintainer activity for 10 months. Demand continues ("this needs to be set as a top priority feature", 2026-03-20). @iceteaSA (2026-08-29) rebased it into [#46109](https://github.com/anomalyco/opencode/pull/46109) for their fork: *"opened mine because our fork needed it on current dev. If yours lands, I'll close mine."* **Cause: internal design-process hold ("designer had some opinions"), never resolved.**

5. **[#26292](https://github.com/anomalyco/opencode/pull/26292) — "feat(opencode): add LLM provider fallback chain"** (closes #7602). Opened 2026-05-08 by @j3k0; **open ~5 months**; `DIRTY`; 31 files: `fallbacks`/`cooldown_seconds` config, cooldown manager, stream-error detection (HTTP 200 with error body), `retry-after` parsing, quota-aware 6h cooldowns, per-agent chains; production-tested by author; explicit comparison vs 5 competing PRs (#24369, #26192, #24013, #18443, #13189 — the last closed-stale).
   **Stall evidence:** author abandoned 2026-06-15: *"After a month of rebasing my PR with almost guaranteed conflicts, I gave up and switched to oh-my-pi (https://omp.sh) … I'll keep that PR open just in case the team ever look at it. I understand they have a thousands PR now, so it's probably lost."* A commenter asserted *"this is outside the scope of this project. For this use case, use a dedicated proxy"* (2026-07-21; commenter @ceoro9, maintainer status **UNVERIFIED**), prompting pushback: *"How on earth is being able to assign provider fallbacks not in scope?"* (2026-09-29). No maintainer ruling on record. **Cause: rebase churn on fast-moving `dev` + contested scope; effectively abandoned upstream.**

6. **Additional open candidates (cause: unknown — no maintainer comment observed in search metadata):** [#29217](https://github.com/anomalyco/opencode/pull/29217) "feat(tui): Add inline $skill invocations with SKILL pill + pasteText support" (open since 2026-05-25, top-reaction PR list); [#20491](https://github.com/anomalyco/opencode/pull/20491) and [#9164](https://github.com/anomalyco/opencode/pull/9164) — **duplicate** "feat(opencode): add Kiro provider" PRs both left open (since 2026-04-01 and 2026-01-18), evidence of triage neglect; [#39015](https://github.com/anomalyco/opencode/pull/39015) "feat: add model-gated auto-approve mode" (2026-07-26); [#31638](https://github.com/anomalyco/opencode/pull/31638) "fix(session): avoid full history hydration after compaction" (2026-06-10); [#8963](https://github.com/anomalyco/opencode/pull/8963) "feat: add directory override env vars and OPENCODE_APPNAME for portable mode and profile isolation" (2026-01-16 — directly addresses `OPENCODE_DATA_DIR`-style requests in #14970); [#41711](https://github.com/anomalyco/opencode/pull/41711) "feat(storage): add safe database maintenance controls" (2026-08-11).

7. **PRs killed by automation (not stalled — closed unmerged):** [#39473](https://github.com/anomalyco/opencode/pull/39473) "fix: retry truncated provider streams" (opened 2026-07-29 by @Davide95 — the same author had told #37852 *"I've already tested it with ~10 candidates, who reported it solves their issue"* — **closed 2026-08-29, label `automated-pr-cleanup`**); [#43881](https://github.com/anomalyco/opencode/pull/43881) "fix(session): retry empty unknown-finish streams instead of stopping" (closed 2026-09-21, `automated-pr-cleanup`); [#42566](https://github.com/anomalyco/opencode/pull/42566) "fix(llm): handle truncated OpenAI tool arguments" (closed 2026-09-14, `automated-pr-cleanup`); [#40032](https://github.com/anomalyco/opencode/pull/40032) "fix(opencode): fail runs when the model stream ends without a finish frame" (closed 2026-08-01); [#44611](https://github.com/anomalyco/opencode/pull/44611) "fix(opencode): retry empty OpenRouter streams" (closed 2026-08-24). All five address the widely reproduced #37852 silent-stop bug. (States verified via `is:pr 39473` search, 2026-10-07.)

### Fork day-1 cherry-pick / upstream-candidate list

| Priority | Item | Value |
|----------|------|-------|
| P0 | PR #26292 (+ siblings #24369/#26192/#24013/#18443 for cross-checking) | Native multi-provider failover with cooldowns — closes top-reaction issue #7602; matches fork's multi-provider goals; author-tested; abandoned upstream |
| P0 | PR #39473 / #43881 (resurrect auto-closed) | Retry truncated/empty streams — fixes silent-stop class (#37852) that breaks subagent orchestration |
| P0 | PR #5422 (or its community fork `johnnymo87/opencode-cached`) | Provider cache config — measured 44–73% token-cost reduction; founder declined on configurability grounds |
| P1 | PR stack #12730/#12731/#12732 (agent teams) | Complete, tested multi-agent messaging/teams implementation (~11.7k lines) — core substrate for Epistemic Swarm; 8 months unmaintained upstream (merge conflicts expected) |
| P1 | PR #4865 / #46109 (subagent sidebar) | Highest-demand TUI orchestration view; two independent implementations exist |
| P1 | PR #8963 (dir-override env vars incl. data-dir isolation) + #41711 (DB maintenance controls) | Mitigates the #14970 SQLite corruption class (per-process data dirs, integrity tooling) |
| P2 | PR #5245 (OTel) | Observability for fleet/swarm operations |
| P2 | PR #31638 (skip full history hydration post-compaction), #14743 (Anthropic cache-hit-rate), #39015 (model-gated auto-approve), #37314 (orphan sub-session cleanup), #29217 (inline $skill) | Targeted fixes aligned with compaction/perf/permission/agent-UX goals |

---

## 4. Bottleneck synthesis — where upstream maintainership diverges from fork goals

Evidence-based, each point anchored:

1. **Automation over stewardship.** Issues auto-close after 60 days of inactivity — repo bot comment on #11012 (2026-07-06): *"To stay organized issues are automatically closed after 60 days of no activity. If the issue is still relevant please open a new one."* Result: substantive design critiques get closed `NOT_PLANNED` without maintainer judgment (#11012) and long-horizon feature threads die silently. A labeled `automated-pr-cleanup` sweep **closed tested bug-fix PRs** (#39473, #43881, #42566) for a still-open, widely reproduced defect (#37852). Fork implication: a triage process that treats abandoned-but-valid patches as assets.

2. **Contribution process gates without throughput.** Every PR is hit by an "issue-first policy" bot demanding a linked issue (PR #12730 comment 2026-02-08, citing CONTRIBUTING.md), yet large reviewed PRs sit 5–10+ months and end `DIRTY` (bit-rot) — #12730, #5245, #5422, #4865, #26292. Author testimony: *"After a month of rebasing my PR with almost guaranteed conflicts, I gave up"* (#26292, 2026-06-15); *"they have a thousands PR now, so it's probably lost."*

3. **Scope philosophy narrower than the fork's.** On provider failover a community voice called it *"outside the scope of this project … use a dedicated proxy"* (#26292, 2026-07-21; maintainer status unverified) while users countered *"The entire project is about being able to use whatever provider you want"* (2026-09-29). On caching config, founder @thdxr: *"i don't know if we want to go as far as making all this deeply configurable yet"* (#5422, 2025-12-12) — and the promised post-refactor pass never happened. The fork's premise (opinionated power features: failover, cache control, deep configuration) is exactly what upstream deferred.

4. **Multi-agent is acknowledged demand but deprioritized.** #11012 (subagent enclosure) closed `NOT_PLANNED` despite 14 sibling requests (#5887 true async delegation, #9280 nested subagents, #7296 configurable delegation, #3023 A2A protocol…); #17311 (orchestration) and #20368 (coordinator shell) closed; the full agent-teams stack #12711/#12730–12732 has run 8 months with zero maintainer engagement. Direct subagent chat was even *removed* after briefly existing (#11012 comment: *"For a month or so it was possible to communicate with subagents… It was really sad seeing subagents reduced to what they are now"*). This is the single widest gap versus the fork's swarm/consensus goals (IUMBTEMS): upstream's architecture keeps subagents as one-shot, fire-and-forget task calls, and its maintainership has not accepted any of the four independent community implementations of teams/messaging.

5. **Design-process opacity as a merge blocker.** #4865 held on *"our designer had some opinions … once we have UX thought out"* (rekram1-node, 2025-12-02) with no visible design output for 10 months; #5422 held on an internal refactor ("llm-centralization") whose merge did not unblock it. Community work is gated on internal processes that produce no public artifacts.

6. **Reliability deficit at the persistence/streaming core.** Silent-failure classes are systemic and long-lived: SQLite corruption with total history loss on shared/NFS/container-mounted homes (#14970, open since 2026-02 with recurring incidents through 2026-09 and no upstream fix — commenters supply their own patches); permanent brick on malformed session data (#49221); provider stream drop recorded as clean completion (#37852, fixes auto-closed); runaway loops burning millions of tokens with no guard (#45442, #49042, #47253, #48827). Upstream ships features fast (v1.18.x → 2.x churn visible in labels `2.0`) while these correctness issues persist — a straightforward fork wedge: single-writer storage / per-process isolation, integrity probes, stream-finalization contract (finish-reason + usage required), and loop guards.

7. **TUI stability churn.** Content-freezing TreeSitter crash loop still unfixed across v1.18.15→v1.18.31 and three OSes (#42264), despite a complete root-cause analysis in the issue; blank-screen/black-screen clusters (#3776, #37803) closed while recurrence continues in new forms (ResizeObserver storms #48633/#43355). High-demand UX affordances languish: session search (#4714, open 11 months), keybinds (#4997), vim motions (#1764/#11111 closed), clickable links (#1168, open 14 months).

**Net assessment.** Upstream's maintainer capacity is absorbed by a 53k+ issue/PR firehose and a fast 2.0 rewrite; triage is heavily automated (auto-close, cleanup sweeps, LLM duplicate-detection bots); and the founder's stated preferences (less configurability, subagent enclosure, provider concerns pushed to proxies) diverge from the fork's goals (multi-provider resilience, deep config, swarm/multi-agent, reliability engineering). The highest-leverage fork-day-1 material is not speculative: it is the pile of reviewed-but-unmerged community PRs (§3) plus the open correctness bugs whose fixes were auto-closed.

---

## Sources

Upstream/stats:
- https://github.com/anomalyco/opencode (repo_view, 2026-10-07: 212,153 stars, 28,254 forks, default branch dev)
- https://github.com/sst/opencode (repo_view resolves to anomalyco/opencode, 2026-10-07)
- https://github.com/Heretek-AI/opencode (repo_view, 2026-10-07: Fork: true); local `git remote -v` (origin https://github.com/Heretek-AI/opencode, branch v2)

Issues (all https://github.com/anomalyco/opencode/issues/<N>): 239, 631, 916 (context only), 1168, 1764, 2206, 2242, 2378, 2484 (context only), 2906, 2945, 3023, 3031, 31485, 3776, 3905, 3935, 4251, 4422, 4714, 4997, 5241, 5386, 5517, 5887, 6299, 6330, 6478, 6491, 6535, 6584, 6680, 6792, 6907, 7296, 7301, 7602, 8868 (context only), 9280, 9511, 9649, 10238, 10374, 10802, 11012, 11111, 11497, 12234, 12661, 12711, 13672, 15388, 15533, 16512, 17311, 17793, 18108, 19094, 19471, 20368, 20695, 20935, 21296, 23914, 25129, 25664, 27027, 27924, 30116, 30611, 31307, 31386, 31764, 32132, 32366, 33126, 33742, 35892, 36423, 36670, 36682, 37353, 37793, 37794, 37803, 37821, 37852, 38246, 38458, 38526, 39001, 39368, 39456, 39628, 39656, 39931, 40169, 40468, 41265, 41354, 41469, 41477, 41678, 41698, 42260, 42264, 42341, 42376, 42502, 42513, 42765, 42950, 42977, 42979, 43267, 43277, 43336, 43355, 43620, 43786, 43996, 44931, 45242, 45442, 45871, 45989, 46398, 47039, 47253, 47613, 47995, 47996, 48051, 48155, 48306, 48416, 48633, 48827, 49039, 49042, 49221, 49347, 49367, 49671, 49842, 49965, 50160, 50751, 50884, 51113, 51503, 51544, 51859, 52016, 52226, 52259, 52288, 52463, 52795, 52860, 52867, 53030, 53146, 53165, 53602, 53611, 53704 (via search listing), 1168, 6152, 6231, 4283 (top-reaction list).

Pull requests (all https://github.com/anomalyco/opencode/pull/<N>): 2735, 4564 (via comments), 4865, 5245, 5422, 5762 (via comment), 6629 (via comment), 7625, 7756 (via #12711 body), 8963, 9164, 9365, 11345, 11684 (via #41265), 12268, 12490, 12721, 12730, 12731, 12732, 13189, 13593, 13860, 14287, 14743, 15926, 16981, 17198, 18443, 20491, 21002, 22674, 24013, 24369, 26192, 26292, 28326, 29217, 29789, 29977, 31638, 34947, 35119, 37314, 38283, 39015, 39473, 40032, 40531 (via #37852 comments), 41711, 42566, 42771, 43881, 44611, 44712, 46109, 47783, 48716, 49558, 50020, 50490, 52610, 53654.

Quoted comments (verbatim, from `issue://`/`pr://` full reads):
- @thdxr on PR #5422, 2025-12-12: "we probably can't explicitly accept this PR (llm-centralization branch) … i don't know if we want to go as far as making all this deeply configurable yet" — https://github.com/anomalyco/opencode/pull/5422#issuecomment-3647978270
- @ormandj on PR #5422, 2026-01-28: "the maintainers made it clear when I first submitted this they had other plans … it wouldn't be accepted even if I did" — https://github.com/anomalyco/opencode/pull/5422#issuecomment-3808498315
- @rekram1-node on PR #4865, 2025-12-02: "our designer had some opinions on this, I will try to get this in once we have UX thought out" — https://github.com/anomalyco/opencode/pull/4865#issuecomment-3603085746
- @rekram1-node on PR #5245, 2025-12-08: "I think opencode already emits telemetry spans if you set: experimental_opentelemetry to true" — https://github.com/anomalyco/opencode/pull/5245#issuecomment-3627809279 (contradicted: #issuecomment-3715171341, #issuecomment-4113366733)
- @j3k0 on PR #26292, 2026-06-15: "After a month of rebasing my PR with almost guaranteed conflicts, I gave up and switched to oh-my-pi … probably lost" — https://github.com/anomalyco/opencode/pull/26292#issuecomment-4704907119
- @ceoro9 on PR #26292, 2026-07-21: "this is outside the scope of this project. For this use case, use a dedicated proxy" — https://github.com/anomalyco/opencode/pull/26292#issuecomment-5036420874 (maintainer status UNVERIFIED)
- GitHub-actions bot on issue #11012, 2026-07-06: "issues are automatically closed after 60 days of no activity" — https://github.com/anomalyco/opencode/issues/11012#issuecomment-4888745777
- GitHub-actions bot on PR #12730, 2026-02-08: "All PRs must reference an existing issue" (CONTRIBUTING.md issue-first policy) — https://github.com/anomalyco/opencode/pull/12730#issuecomment-3867803959
- @iceteaSA on PR #4865, 2026-08-29 (fork builds on stalled PR): https://github.com/anomalyco/opencode/pull/4865#issuecomment-5461063350
- @anthonykewl20 on issue #39931, 2026-08-13 (community fork fix, commit 2d0950f6e5 "ranex-trim"): https://github.com/anomalyco/opencode/issues/39931#issuecomment-5284986565
- @illethyus on issue #39931, 2026-08-09: "the bash allowlist is advisory only, not a security boundary" — https://github.com/anomalyco/opencode/issues/39931#issuecomment-5232898972
- @Davide95 on issue #37852, 2026-08-05 (tested fix PR #39473): https://github.com/anomalyco/opencode/issues/37852#issuecomment-5188808702
