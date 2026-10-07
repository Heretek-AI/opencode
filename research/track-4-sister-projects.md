# Track 4 — Sister Ecosystem: IUMBTEMS (Epistemic Swarm) & scraper-swarm

Date: 2026-10-07. Author: SisterTrack (research only; no builds/tests run).
Primary sources are local on-disk checkouts (`/home/john/Projects/IUMBTEMS`, `/home/john/Projects/scraper-swarm`, `/home/john/Projects/opencode`), cross-checked against GitHub (`xd://github` repo_view/search_repos) and the npm registry. Every factual claim below carries a file path (with line numbers where cheap), a GitHub URL, or a registry URL. Labels: **VERIFIED** (primary source seen), **NOT FOUND** (searched, absent — search described), **UNVERIFIED** (plausible but not evidenced).

---

## 0. Claim-verification table (mission-brief component claims)

| Claim (from brief) | Verdict | Evidence |
| --- | --- | --- |
| IUMBTEMS = "AI Build Factory for OpenCode v2" | **VERIFIED** (as self-description and implementation) | `IUMBTEMS/README.md:3-5` ("Epistemic Swarm is the product: an AI build factory for OpenCode v2"); implemented by `packages/core/src/factory/machine.ts` + `packages/opencode/` plugin |
| Workflow state machine `Grill → Research → Spec → Build ⇄ QA → Release` | **VERIFIED** | `packages/core/src/factory/machine.ts:1` ("GRILL → RESEARCH → SPEC → BUILD ⇄ QA → RELEASE → DONE"); `packages/core/src/factory/state.ts:3` (`StageSchema = z.enum(["GRILL","RESEARCH","SPEC","BUILD","QA","RELEASE","DONE","HALTED"])`) |
| Workflow states/transitions/gating implemented in code | **VERIFIED** | `machine.ts:245-248` (`requireStage`), transitions `begin` :258, `beginResearch` :321, `completeResearch` :336, `startBuild` :434, `complete` :538, `qaVerdict` :663, `tiebreak` :677, `passPhase` :730, `failPhase` :757, `replan` :785, `release` :1009 |
| Mechanical gating: tools refuse out-of-order calls | **VERIFIED** | `machine.ts:245-248` throws `FactoryError("Not allowed in stage …")` on every transition; surfaced as model-facing errors via `packages/opencode/src/tools.ts:66-70` (`ToolError`); seat checks `machine.ts` `requireSeat` (e.g. :539, :664, :913) + `packages/core/src/agents/registry.ts:403` (`seatMayUse`) |
| Human-in-the-loop signing/approval | **VERIFIED (real code)** | `packages/core/src/approval/record.ts:1-6` (human-only approvals; "no agent tool or MCP tool does"), `approval/keystore.ts` (HMAC-SHA256 signing key, 0600, `signRecord`/`verifyRecordMac`/`confirmationCode`), CLI dialog `packages/cli/src/human.ts:33-66`, TUI dialog `packages/opencode/src/tui.tsx:23-63,118-123` (`/es-approve`), preview-redeem token flow `packages/opencode/src/rpc.ts:183-227` |
| Evidence verification (cited diffs/test passes before phase advancement) | **VERIFIED (mechanical for tests/gates; ADVISORY for citation style)** | Mechanical: red-first evidence rules `machine.ts:555-572` (`tdd/test-change`, `tdd/red-first` findings), acceptance-criteria check `machine.ts:622-652` (`checkCriteria`), full gates before commit `machine.ts:573-580`; research evidence: `packages/core/src/claims/witness.ts:1-51` (verbatim-quote + code-location witness), `packages/core/src/research/auditor.ts:1-16` (claim tag audit). ADVISORY: QA verdict notes are instructed to be "concrete, numbered notes (file:line, what, fix)" in the tool description (`packages/core/src/ops/tools.ts:227-231`) but `qaVerdict` records notes verbatim without format validation (`machine.ts:663-677`) |
| Dual-QA (independent auditor agent) | **VERIFIED (real code)** | Dual QA seats `es-qa-functional` / `es-qa-adversarial` (`agents/registry.ts:267-298`), both verdicts required (`machine.ts:700-704` `qaOutcome`: "if (!functional \|\| !adversarial) return undefined"), manager tiebreak on split (`machine.ts:677-702`). Dual auditors `es-auditor-thesis` / `es-auditor-antithesis` (`agents/registry.ts:335-366`), `openAudit`/`auditVerdict`/`auditTiebreak` (`machine.ts:813-975`), audits block stage advancement `machine.ts:995-1003` (`requireAuditsClear`), human-only dismissal `machine.ts:977` |
| IUMBTEMS integrates with opencode via plugin | **VERIFIED** | `packages/opencode/src/server.ts:26` (`Plugin.define({ id: "epistemic-swarm" … })` — `@opencode/plugin` 2.0.24 per `packages/opencode/package.json`), TUI plugin `packages/opencode/src/tui.tsx:6` (`@opencode/plugin/tui`); plus `packages/cli` `es` CLI + stdio MCP server for non-OpenCode harnesses (`packages/cli/src/mcp.ts:1-6`) |
| scraper-swarm: `swarmd` | **VERIFIED** | `apps/swarmd/src/swarmd/` (server.py, reconciler.py, docker_engine.py, catalog.py, policy.py, render.py, updater.py) |
| scraper-swarm: panel API | **VERIFIED** | `apps/panel-api/src/panel_api/` (main.py, routers/{agents,auth,security,services}.py, vault.py, swarmd_client.py) + `apps/panel-web/` (React/Vite console) |
| scraper-swarm: `@scraper-swarm/opencode-plugin` | **VERIFIED (name) but see §4.6 caveats** | `packages/opencode-plugin/package.json` (`"name": "@scraper-swarm/opencode-plugin"`, v0.1.0), `src/index.ts`, `src/index.test.ts` |
| scraper-swarm: distributed scraping | **PARTIAL — term NOT FOUND** | The literal phrase "distributed scrap*" appears nowhere in `/home/john/Projects/scraper-swarm` (searched `*.py,*.md,*.ts,*.tsx,*.yaml,*.toml` excluding node_modules/.venv — zero hits). The substance exists as a multi-engine scraping cluster: 10 engine definitions `catalog/services/*.yaml` (cloakhq, crawl4ai, cyberscraper, egress-web, firecrawl, gpt-researcher, maxun, scrapling, searxng, valkey), orchestrated by swarmd Docker reconciliation (`apps/swarmd/src/swarmd/reconciler.py`) |
| scraper-swarm: live intelligence | **NOT FOUND** | No occurrence of "live intelligence" or "intelligence" as a component in the repo (same search, zero hits). Closest real artifacts: live end-to-end smoke phase `.roadmap/01-live-smoke/GOAL.md`, live health-gated reconciler (`catalog/services/valkey.yaml` health probe; `docs/opencode-integration.md` §3 latency-bearing `/mcp` calls), and the Workbench live console (`apps/panel-web/src/components/workbench/WorkbenchView.tsx`) |
| scraper-swarm: caching/ingestion of docs | **NOT FOUND in scraper-swarm; the doc-cache is IUMBTEMS's** | No cache/ingest module in `apps/gateway/src/gateway/server.py` (grep "cache\|valkey\|ingest": zero hits; Valkey is only a catalog service and a future rate-limit bucket seam — `docs/opencode-integration.md` §3 "Multi-replica deployments … should move the bucket to Valkey; the gateway helper (`_check_rate_limit`) is the seam"). Content-addressed doc ingestion/caching lives in IUMBTEMS: `packages/core/src/research/cache.ts` (SourceCache), `packages/opencode/src/research.ts:1-45` (websearch results cached `sha256:` + host webfetch cached) |

---

## 1. IUMBTEMS ("Epistemic Swarm") architecture

### 1.1 What it is

TypeScript monorepo (Bun/Node ≥ 22, "No Python" — `README.md` Development section) implementing an "AI build factory for OpenCode v2" (`README.md:3-5`). npm package `@heretek-ai/epistemic-swarm` **is published**: npm registry `https://registry.npmjs.org/@heretek-ai/epistemic-swarm` shows `dist-tags.latest: 1.0.5` (checked 2026-10-07). GitHub: `https://github.com/Heretek-AI/IUMBTEMS`, public, default branch `main`, primary language TypeScript, updated 2026-10-07 (repo_view). Note: `README.md:7-10` says "Status: 1.0 in development … the packages publish to npm" after the `rewrite`-branch cutover; the registry already carries 1.0.5, so publishing has started (registry is authoritative for what is published; README lags).

Repo layout (README "Repository layout" table, confirmed on disk):
- `packages/core` — harness-neutral engine: `src/factory/` (state machine), `src/gates/` (mechanical gates), `src/approval/`, `src/audit/`, `src/trust/`, `src/claims/`, `src/research/`, `src/brainstorm/`, `src/harvest/`, `src/queereye/`, `src/lsp/`, `src/hooks/`, `src/agents/registry.ts`, `src/ops/tools.ts`.
- `packages/opencode` — the OpenCode plugin (server + TUI entrypoints).
- `packages/cli` — `es` CLI + coarse stdio MCP server (`src/mcp.ts`).
- `packages/testkit` — in-process host testing; `schemas/` — 50+ generated JSON Schemas; `evals/` — 6 recorded eval cases (`evals/cases/{audit-fires,brainstorm,darkharvest-fires,factory,grill,scout-fires}.json`).
- `review/opencode/` — a full git clone of `https://github.com/anomalyco/opencode.git` (upstream; last commit `4a0f273fa7 2026-10-06`) kept for review/audit (e.g. `review/opencode/V2_HTTP_API_AUDIT.md`, `review/opencode/specs/v2/*.md`), not product code.

### 1.2 Real workflow/state-machine implementation

**Files:** `packages/core/src/factory/machine.ts` (1,200+ lines, the `Factory` class), `state.ts` (Zod `FactoryStateSchema`), `tree.ts` (grill design tree), `journal.ts`, `summary.ts`; operations exposed to models as tools in `packages/core/src/ops/tools.ts:67-277` (`esTools`).

**States:** `state.ts:3` — `GRILL | RESEARCH | SPEC | BUILD | QA | RELEASE | DONE | HALTED`. Header `machine.ts:1` documents the loop "GRILL → RESEARCH → SPEC → BUILD ⇄ QA → RELEASE → DONE".

**Transitions (all in `machine.ts`, each guarded):**
- `begin` :258 — starts a run only from fresh GRILL (`:262-263` refuses if a run exists).
- GRILL: `writeFrontier` :277 — grill seat saves the design tree; nodes are never deleted (validation at :285-292).
- `beginResearch` :321 — GRILL→RESEARCH **only after human frontier approval**: `verifyApproval(this.root, "frontier", …)` at :325-326.
- `completeResearch` :336 — RESEARCH→SPEC only if `.factory/research/REPORT.md` exists (:343), every grill-deferred fact is answered (:363), domain pack loads (:377), and the research dossier scores (audit line :395: "Verified …, rejected …, negative knowledge …, inferred …, hypotheses …").
- `startBuild` :434 — SPEC→BUILD only after human **spec** approval (:339 `verifyApproval(this.root, "spec")`), roadmap within `maxPhases` (:443), git repo present (:441); creates run branch `factory/<runId>/integration` (:457) and per-phase worktrees (`activate` :481).
- BUILD→QA: `complete` :538 (programmer seat only, :539) — see §1.5.
- QA→next phase: `qaVerdict` :663 records each seat's verdict; `settleQa` :704 needs both seats (+ manager `tiebreak` :677 on split); `passPhase` :730 enforces fast-forward merge only (`:734-737` refuses non-descendant commits) then activates the next phase or moves to RELEASE (:748-752). `failPhase` :757 increments failures, resets QA, re-opens audit; third failure allows a one-shot manager `replan` :785 (GOAL.md must change, :795).
- RELEASE→DONE: `release` :1009 runs release gates (:1028) and opens a **draft PR for a human to merge** (`ops/tools.ts:267-277` description; `prBody` :1055; human records PR URL `recordPr` :1074).
- HALTED: `guard` :213-241 trips on spend-ceiling overrun (:225-228) and autonomy runtime overrun (:230-233); state is saved and every op throws `FactoryHalted` (:200-203).

All state changes go through `mutate` (:189-210) under a state lock, with a save-on-halt guarantee.

### 1.3 Mechanical gating — how out-of-order calls are refused

1. **Stage gate:** `machine.ts:245-248` — `requireStage(state, …stages)` throws `FactoryError("Not allowed in stage ${state.stage} (needs …)")`; every transition calls it first (`beginResearch` :323, `completeResearch` :339, `startBuild` :437, `complete` :541, `qaVerdict` :666, `release` :1012, …). A tool call out of order is a hard error, not a warning.
2. **Seat gate:** `requireSeat` (e.g. `complete` :539 `this.requireSeat(agentId, "programmer")`, `qaVerdict` :664 seats `qa-functional|qa-adversarial`, `auditVerdict` :913 auditor seats only) plus per-agent tool allowlists in `agents/registry.ts:92-382` (each `AgentSpec.tools` list, e.g. programmer `["es_status","es_gates_run","es_complete"]` :257) and `seatMayUse` :403.
3. **Permission layer (host):** each `es_*` tool is registered "under its own permission action so per-agent rules can hide it" (`packages/opencode/src/tools.ts:1-3,59`); refusals become model-facing `ToolError`s (:66-70).
4. **Human-only operations are unreachable from tools:** `approval/record.ts:1-6` — "Only the CLI (TTY + typed code) and the TUI dialog call recordApproval; no agent tool or MCP tool does"; the MCP server explicitly has "no approve, trust, waive or resume tool" (`packages/cli/src/mcp.ts:1-6`).
5. **Artefact tamper gates:** approvals re-hash artefacts so "editing a spec after approval blocks the build" (`approval/record.ts:4-6`); `goalUnchanged` (`machine.ts:513-521`) refuses completion when GOAL.md drifted; `verifyControl` hashes human-authorised control files (`README.md` "Known enforcement limits" — `gates.json`, `config.json`, `frontier.json`, `approvals/**`, `waivers/**`).

README "Known enforcement limits" is unusually candid about the boundaries (bwrap read-only seats with allowlist fallback, control-file baseline scope, strict SPDX licence detection, opt-in evals, advisory MCP caller identity) — useful for the fork's threat model.

### 1.4 Human-in-the-loop signing/approval — what is real today

**Real (shipped code + tests):**
- **Signing:** `approval/keystore.ts` — machine-local HMAC-SHA256 key created 0600 in the user-global state dir ("agents are denied that dir by policy", file header), `signRecord`/`verifyRecordMac` (timing-safe), `signingKeyId` fingerprint, `confirmationCode()` short code humans must type back.
- **Approval records:** `approval/record.ts` — subject derived from factory layout per stage (frontier: `frontier.json`; spec: `roadmap.json` + phase `GOAL.md`s), Zod-validated, hashed (`approvalSubject`), signed records via `recordApproval`; transitions call `verifyApproval` which re-hashes current artefacts.
- **CLI dialog:** `packages/cli/src/human.ts:33-66` (`es approve <frontier|spec>`: preview of exact sha256+paths → `confirmWithCode` TTY code → `recordApproval`); sibling human-only commands trust/waive/resume/rebaseline in the same file.
- **TUI dialog:** `packages/opencode/src/tui.tsx` — human-only dialogs ("Approvals, trust and…", :1); `/es-approve` command (:118-123); confirm flow shows a server-computed preview and "confirming redeems that preview's token" (:3-5) via `rpc.ts` `previewApproval`/`approve` (:183-227) with a preview→redeem token that fails on expiry or subject swap (`rpc.ts:54-56`: "What you confirmed changed since the preview; preview again").
- **Practical proof it fires:** scraper-swarm's own factory run logs show QA/escalation flows (see §4.8) and IUMBTEMS `.legacy-state/roadmap/*/qa-tiebreak-*.md` + `escalation-2026-09-29.md` files record tiebreaks.

**Planned/not yet shipped:** native Claude Code / Pi / Antigravity adapters — `docs/CAPABILITIES.md` marks every `## claude` row `UNSUPPORTED` ("ships in 1.1"–"1.3"), matching `README.md` ("Claude Code (1.1), Pi (1.2) and Antigravity (1.3) adapters follow"). So multi-harness approval parity is roadmap.

### 1.5 Evidence verification & dual-QA — labelled

- **Evidence verification before phase advancement: REAL, mechanical.** `machine.ts` `complete` (:538-620): refuses when the worktree has no changes (`factory/no-changes` :548-553), when no test file changed (`tdd/test-change` :555-560), when no failing test run was recorded first (`tdd/red-first` :561-568, fed by `recordGateRun` :523-536 tracking `tests-red` history events), when any acceptance criterion fails (`checkCriteria` :622-652 — file-contains, test-file-exists, or command exit 0 with 300 s timeout), or when full gates fail (:573-580). Only then does it commit and hand to QA (:585-617).
- **Cited-diff discipline for QA/auditor notes: ADVISORY.** Tool descriptions demand `file:line` notes (`ops/tools.ts:227-231`; audit verdicts similar), but the machine records notes verbatim (`machine.ts:663-677`) — the format is prompt-enforced, not schema-enforced.
- **Research evidence: REAL, mechanical.** `claims/witness.ts:1-51` re-checks claims "against evidence the agent cannot have fabricated": VERIFIED quotes against the content-addressed cache (tamper-refusing `SourceCache.get`, `research/quote.ts` one-quote law) and code locations re-read from disk with verbatim-excerpt matching (`checkLocation` :34-51). `research/auditor.ts:1-16` requires every claim line to carry `[VERIFIED: sha256:… "quote"]` / `[INFERRED]` / `[HYPOTHESIS]` / `[NEGATIVE_KNOWLEDGE]` tags and reports grounded/untagged/ungrounded coverage. A forged source is denied on the host (`docs/CAPABILITIES.md` claims row).
- **Dual-QA: REAL code.** Two independent seats (`es-qa-functional`, `es-qa-adversarial` — `agents/registry.ts:267-298`, both `readonlyShell: true`), both verdicts required (`machine.ts:700-704`), manager-only `es_tiebreak` on disagreement (`ops/tools.ts:238-260`), fail notes from all failing seats concatenated into the failure record (`machine.ts:717-725`).
- **Independent auditor pair: REAL code.** `es-auditor-thesis` / `es-auditor-antithesis` (`registry.ts:335-366`) with `es_audit_verdict`; audits opened only by the factory seat or a human with a spend ceiling (`machine.ts:813-868`); verdicts per seat (`:906-925`), manager tiebreak on split (`:936-953`), settle logic `:955-975`; open audits **block** research/build/release advancement (`requireAuditsClear` `:995-1003`); human-only dismissal with reason (`:977-1006`). Findings must be witnessed on disk — "a hallucinated line is refused" (`docs/CAPABILITIES.md` audit row; witness code above).

### 1.6 How IUMBTEMS integrates with opencode today

**Mechanism: a single OpenCode plugin (server + TUI entrypoints) plus an `es` CLI/stdio-MCP fallback. No core fork required.**

- Plugin: `packages/opencode/src/server.ts:26-…` `Plugin.define({ id: "epistemic-swarm" })` — registers agents via `ctx.agent.transform` (:37-50, "wildcard-deny scoping … never touches built-ins or defaults"), skills via `ctx.skill.transform` (:53-61), tools via `ctx.tool.transform` → `registerTools` (:64), websearch via `ctx.websearch.transform` (:65), policy hooks first "so its denials win over any user hook" (:67-68), hook bridge (`hooks.ts`, Claude hook schema bridged in-process per `docs/CAPABILITIES.md` hooks row), session/spend tracking (`session.ts`), RPC for panels (`rpc.ts`, `rpc-def.ts`). Dependency: `@opencode/plugin` 2.0.24 and `@opencode/sdk` 2.0.24 (`packages/opencode/package.json`).
- TUI: `packages/opencode/src/tui.tsx` (`@opencode/plugin/tui`) — human-only dialogs (`/es-approve`, trust, resume, LSP install) + four session panels (factory dashboard, LSP manager, hook inspector, brainstorm board — `panels.tsx:1-2,213-215` `PANEL_COMPONENTS`), rendered with OpenTUI/Solid.
- Tests: `packages/opencode/test/{plugin,hooks,panels,research,brainstorm,harvest,design,fires,fires-gates,lsp,tui,tui-pack,evals}.test.ts` (13 files); the capability matrix `docs/CAPABILITIES.md` ties every "ENFORCED" row to one of these.
- CLI fallback: `packages/cli/src/mcp.ts:1-6` — coarse stdio MCP (JSON-RPC 2.0, `PROTOCOL_VERSION "2025-06-18"`) exposing `es_*` tools (minus all human-only operations) for harnesses without a native plugin; caller identity is ADVISORY (`README.md` limits).
- Declared integration style: **additive** — "writes no config files and never touches built-in agents or defaults" (`server.ts:1-3`).

---

## 2. scraper-swarm architecture

### 2.1 What it is

Self-hosted "search and scraping engine cluster for AI agents" with "privileged sidecar orchestration, fail-closed SSRF egress isolation, multi-factor authentication, and native OpenCode v2 integration" (`docs/DEPLOYMENT.md:3`). Python 3.12/uv backend + pnpm/Vite front end; monorepo `apps/{gateway,panel-api,panel-web,swarmd}`, `packages/{opencode-plugin,ui-theme}`, `catalog/services/*.yaml`, `deploy/`, `bootstrap/install.sh`, `tests/security/`. GitHub: `https://github.com/Heretek-AI/scraper-swarm`, public, default branch `main`, primary language Python, updated 2026-10-07 (repo_view). No root README (only `docs/`).

Component verdicts: `swarmd` **VERIFIED** (`apps/swarmd/`), panel API **VERIFIED** (`apps/panel-api/`), `@scraper-swarm/opencode-plugin` **VERIFIED** (`packages/opencode-plugin/`), "distributed scraping" **term NOT FOUND / substance partial** and "live intelligence" **NOT FOUND** (§0).

### 2.2 Deployment topology

- **Control plane** (`docs/DEPLOYMENT.md:20-31`): `bootstrap/install.sh` provisions `/var/lib/scraper-swarm` (0700), AES-256-GCM master key (0600), one-time `swarm_boot_…` admin token, then `deploy/docker-compose.control.yml` runs `caddy`, `panel-api`, `swarmd`, `gateway`, `egress-web`.
- **Remote MCP endpoint:** the gateway exposes a single Streamable-HTTP MCP endpoint `POST /mcp` behind Caddy (`apps/gateway/src/gateway/server.py:515-522`, `create_gateway_app` + `@app.post("/mcp")` :522). OpenCode config template in `docs/opencode-integration.md` §2: `"mcp": {"scraper-swarm": {"type": "remote", "url": "https://<gateway>/mcp", "headers": {"Authorization": "Bearer swarm_sec_…"}}}`.
- **Zero-trust ingress options:** Cloudflare Tunnel (`deploy/docker-compose.cloudflare.yml`, `deploy/cloudflared/config.yml.tmpl`; `docs/DEPLOYMENT.md:35-46`) or Tailscale serve/funnel (:48-52) — i.e. the design assumes agents reach the gateway **remotely over HTTPS**, not via local stdio.
- **Egress:** every engine call goes `Agent → /mcp → engine → Smokescreen (CONNECT proxy, built from pinned source — `docs/spikes/m0-findings.md` row 6) → Internet`; SSRF is pre-denied per URL (`deny_reason_for_url`, `server.py:473` with 8 s bound).
- **Engine fleet (catalog-driven):** 10 YAML service definitions in `catalog/services/` (searxng, crawl4ai, firecrawl, scrapling, cloakbrowser, maxun, cyberscraper, gpt-researcher, valkey, egress-web), each with image pinning, hardening (`read_only`, `cap`-style user, tmpfs) and health probes (e.g. `catalog/services/valkey.yaml` health `valkey-cli ping`). swarmd renders and reconciles them (§2.5).

### 2.3 Gateway: MCP surface, scoped keys, rate limiting

`apps/gateway/src/gateway/server.py` (834 lines):
- **Tools exposed over `/mcp`:** `web_search` (SearXNG meta-search, :426-431), `fetch_page` (Crawl4AI → markdown, :456-459), `stealth_scrape` (Scrapling Camoufox anti-bot extraction, :630-642), `deep_research` (GPT Researcher, :~690). `tools/list` at :581-642; `tools/call` dispatch :644+.
- **Scoped agent keys:** `verify_agent_token` (:226-412) validates Bearer `swarm_sec_…` keys or session cookies; per-key `scopes` JSON with required-scope checks (`search` for web_search/deep_research, `scrape` for fetch_page; missing scope → `403` :327-333), expiry → `401` (:234), revocation supported (`DELETE /agents/keys/{id}` — `apps/panel-api/src/panel_api/routers/agents.py:260-261`).
- **Rate limiting:** `_check_rate_limit(key_id, rpm)` (:51) single-process sliding window ("one HTTP call costs one hit" — `docs/opencode-integration.md` §3), default 60 rpm (`rate_limit_rpm or 60` :336), operator sessions 600 rpm (:395-401), search-only sessions 60 rpm (:405-412); over-limit → `429`. Documented seam for multi-replica: move bucket to Valkey (`docs/opencode-integration.md` §3).
- **Audit:** hash-chained agent activity rows (`log_agent_activity` :132, `apps/panel-api/src/panel_api/audit.py`), denied-row flood sampling (`_should_audit_denied` :104-127, cooldown per `(agent, action, status)` — `docs/opencode-integration.md` §5), secret scrubbing (`scrub_secrets_from_text`), redacted targets; verification endpoint `POST /security/audit/verify` returns `{"valid": true, "entries_checked": N}` (§5).

### 2.4 Panel API + console

- `apps/panel-api/src/panel_api/` — FastAPI (`main.py` `create_app` :69) with routers `auth`, `agents`, `security`, `services`; SQLite (`db.py`), AES-256-GCM secrets vault (`vault.py:1-20`; master key env or 0600 file), `swarmd_client.py` (Unix-socket intents to swarmd), SSRF guard mirror (`ssrf_guard.py`).
- **Key issuance:** `POST /agents/keys` (`routers/agents.py:85`) — name (trimmed, case-insensitive unique → `409`), scopes validated (`422` on unknown), default TTL 720 h, never-expire needs `allow_never_expire=true`, placeholder-host guard (`422` unless acked); returns `raw_key` once + `opencode_snippet` ready-to-paste fragment; `GET /agents/keys` (:235) and audit rows show prefix only (`docs/opencode-integration.md` §1).
- `apps/panel-web/` — React console: Dashboard (`OverviewView`), Workbench (interactive `/mcp` JSON-RPC console), Containers, Engine Store (`EngineStoreView` + `EngineConfigModal`), Security, Wizard; login modal + session auth. Workbench is "the equivalent interactive path" for tool calls (all traffic through gateway auth/scope/SSRF/audit — `docs/opencode-integration.md` §4).

### 2.5 swarmd (privileged sidecar orchestrator)

`apps/swarmd/src/swarmd/` — Unix-domain-socket intent server (`server.py:1-16`) consumed by panel-api; **reconciliation** is the core: background loop + `reconcile_now` / `get_reconcile_status` intents repair "DB-vs-Docker drift (DB `running` vs 0 containers)"; desired state `wanted.json` persists across restarts (`reconciler.py`, 526 lines). Plus `docker_engine.py` (Docker orchestration), `catalog.py` (service catalog loader), `render.py` (compose rendering), `policy.py` (render policy), `updater.py` (health-gated updates). Tests: `apps/swarmd/tests/{test_live_docker,test_reconcile,test_render_policy}.py`.

### 2.6 `@scraper-swarm/opencode-plugin` — actual API surface

`packages/opencode-plugin/src/index.ts` (162 lines, v0.1.0):
- Exports `ScraperSwarmPlugin` (default) taking a self-declared `PluginContext` (`project/client/$/directory/worktree` — :46-52), returning **two hooks**: `"tool.execute.before"` (:145-162) and `"command.executed"` (:164-168, `/swarm-status` prints a status line).
- `tool.execute.before` **blocks** (throws) rather than redirects: normalized tool-name blocklist (`BLOCKED_TOOL_SUBSTRINGS` fetch/curl/wget/python/powershell/pwsh/httprequest/socket/netcat + exact `cmd/nc/ncat`, :66-77) and shell-arg exfil heuristics (`shellArgsLookLikeExfil` :88-127: `/dev/tcp`, `socket`, `base64`, `invoke-webrequest`, bare `nc`/`cmd` word-boundary regexes). Generic messages never echo URLs (:129-135). Residual risk (novel/obfuscated exfil) is documented and deferred to gateway SSRF + Smokescreen (file header :33-38; `docs/opencode-integration.md` §4).
- Unit tests: `src/index.test.ts` (vitest; asserts the `swarm_fetch` message).
- **Caveats found (flag, not inferred):**
  1. `package.json` has **no dependency on `@opencode/plugin`** and defines its own `PluginContext`; the hook names `tool.execute.before` / `command.executed` match the older documented hook surface that `docs/spikes/m0-findings.md` row 2 researched ("tool.execute.before can mutate output.args or throw"), not the v2 `Plugin.define` + `State.Transformable` API that IUMBTEMS uses against `@opencode/plugin` 2.0.24 (`IUMBTEMS/packages/opencode/package.json`). Compatibility with the fork's actual plugin host (`opencode/packages/core/src/plugin/hooks.ts` — `ToolHooks`, "only tool execute.before may fail") needs a runtime check before adoption. UNVERIFIED at runtime (no builds run per mission rules).
  2. The block message directs agents to "`swarm_fetch` / scraper-swarm MCP tool" (`index.ts:131`), and `docs/DEPLOYMENT.md:114` names "`swarm_fetch` and `swarm_search` MCP tools", but the gateway registers **`web_search`, `fetch_page`, `stealth_scrape`, `deep_research`** (`server.py:426,456,630` + `tools/list` :581-642). `swarm_fetch`/`swarm_search` exist nowhere in `apps/` (grep across `*.py`: zero hits; only docs/plugin/tests mention them). Naming inconsistency to resolve in the fork.
  3. The plugin is a **guard only** — it supplies no MCP client, no tools, no config; the MCP connection is the separate `mcp` config block (§2.2).

### 2.7 Caching/ingestion

See §0 last row: scraper-swarm has **no docs cache/ingestion** (Valkey is a catalog service + future rate-limit seam). The doc ingestion/caching stack is IUMBTEMS's (content-addressed SourceCache + citable websearch/webfetch caching, `IUMBTEMS/packages/opencode/src/research.ts:1-45`). For the fork, the natural combination is scraper-swarm as transport + IUMBTEMS cache as evidence store.

### 2.8 scraper-swarm is itself a factory product (dogfooding evidence)

`.factory/scraper-swarm-factory/state.json` records factory runs with dual QA seats `qa-a`/`qa-b`, verdicts, retries and escalations (e.g. `"01-live-smoke"`, 4 QA reports, "status": "escalated"); `.roadmap/{01-live-smoke,02-infra-reconcile,03-opencode-integration}/` hold `GOAL.md`/`phase.json`/`dossier.json`. Recent commits are literally factory artifacts ("Phase 03 retry2 QA-B P0s …", `git log`: `3314cf6 2026-10-06`, `8f53727 2026-10-06`). Note the state schema (`qa-a/qa-b`, `gate_cycles`) matches the **legacy Python** harness format, not IUMBTEMS 1.1's `.factory/runtime/state.json` (`FactoryStateSchema` with `Stage`) — i.e. this repo was built under the older Epistemic Swarm factory. Either way, dual-QA + escalation is demonstrated on real output.

---

## 3. Integration blueprint — sister capabilities onto the OpenCode fork

Seam symbols below are from `/home/john/Projects/opencode` (branch `v2` locally; GitHub `Heretek-AI/opencode`, public fork of `anomalyco/opencode`, default branch `dev`, updated 2026-10-07 — repo_view). The fork is Effect-based; the plugin host ships **promise**, **effect** and **tui** surfaces (`packages/plugin/src/{promise,effect,tui}/`).

| # | Sister capability | Mechanism in the fork | Seam point (file/symbol) | Invasiveness |
| --- | --- | --- | --- | --- |
| 1 | IUMBTEMS factory (Grill→…→Release) as first-class agents/tools | Load `@heretek-ai/epistemic-swarm` via the plugin config; agents via agent transform, `es_*` tools via tool transform | Plugin loading: `packages/core/src/plugin/module.ts:3-40` (via `@opencode/plugin/source`); promise plugin API `packages/plugin/src/promise/plugin.ts` (`Plugin.define`); transforms are `State.Transformable<Editor>` — tool `Editor.add/update/remove` `packages/core/src/tool.ts:26-32`; agent/skill/command transforms `packages/core/src/{agent,skill,command}.ts` + `packages/core/src/plugin/{agent,skill}.ts` | **Plugin-only** (this is exactly what `IUMBTEMS/packages/opencode/src/server.ts` already does against `@opencode/plugin` 2.0.24; the fork's `packages/plugin` is that package) |
| 2 | Per-tool permission actions & seat isolation (IUMBTEMS "each tool under its own permission action", wildcard-deny agents) | Permission ruleset per agent; missing permissions default deny | `packages/core/src/permission.ts:1-38` (`Permission.Service`, `Request`/`Reply`, `missingAgentPermissions = [{action:"*",resource:"*",effect:"deny"}]` :22); persistence `packages/core/src/permission/{saved,sql}.ts`; plugin hook `packages/plugin/src/{promise,effect}/permission.ts`; tool snapshot takes a ruleset `packages/core/src/tool.ts:39` (`snapshot(permissions?: Permission.Ruleset)`) | **Plugin-only** (config-level rules) unless new permission verbs are wanted → small core patch in `@opencode/schema/permission` |
| 3 | scraper-swarm gateway as the agent's web tools (remote MCP, Bearer keys) | `mcp` config block, remote type, custom headers | MCP client `packages/core/src/mcp/client.ts:272` (`requestInit: config.headers ? { headers: config.headers }` — passes `Authorization: Bearer swarm_sec_…` verbatim), transport/registry `packages/core/src/mcp/{index,stdio,oauth,instructions}.ts`; MCP→tool bridge `packages/core/src/tool/mcp.ts:17-18,30-45` (namespaced tool names `${server}_${tool}`, registered via `tools.transform`, one permission action per tool); config validation `packages/core/src/config/normalize.ts:264-266,560-567` (`mcp` + `options.headers`) | **Plugin-only / config-only** — zero core changes; matches the snippet in `scraper-swarm/docs/opencode-integration.md` §2 |
| 4 | scraper-swarm fetch-guard (block raw fetch/exfil, steer to swarm tools) | `tool.execute.before` may reject a call with `Tool.Error` | Hook domain `packages/core/src/plugin/hooks.ts:17-25` ("Only tool execute.before may fail: a Tool.Error rejects the call before it runs") → the guard's throw-blocks pattern is supported as-is | **Plugin-only** today (block+message). Actual *re-routing* (rename `webfetch`→`fetch_page`) would need a core change in tool dispatch (`packages/core/src/session/…` tool call path / `ToolHooks` failure channel) — flag as possible fork feature |
| 5 | IUMBTEMS evidence cache + claim witness as citable research inside sessions | Content-addressed cache + websearch/webfetch interception | websearch transform seam `packages/core/src/plugin/websearch/` (+ `packages/core/src/websearch.ts`); IUMBTEMS already wraps it (`IUMBTEMS/packages/opencode/src/research.ts` `registerWebsearch`, `createWebfetchCache`); storage via plugin storage `packages/plugin/src/{promise,effect}/storage.ts` or repo-local `.factory/` | **Plugin-only** |
| 6 | Human-only approval dialogs (/es-approve) | TUI plugin surface + RPC | `packages/plugin/src/tui/{plugin,context,solid}.ts` (Solid/OpenTUI context); IUMBTEMS consumes it (`IUMBTEMS/packages/opencode/src/tui.tsx` `context.ui.dialog.confirm`, `rpc.ts` preview→redeem). For new panels/stories: `packages/tui/src/feature-plugins/system/storybook` (per `AGENTS.md:16-18` "add a fixture-driven story … and register it in `index.tsx`") | **Plugin-only** for dialogs; **fork-owned** for first-class storybook stories (TUI package) |
| 7 | Dual-QA / dual-auditor + gated phases as an opinionated Prime mode (auto-run the factory loop) | Subagent spawning + session/command surface | Agent `spawns` depth-1 model already exists in IUMBTEMS registry (`agents/registry.ts` `spawns`); fork side: subagent invocation & session control `packages/core/src/session/`, commands `packages/core/src/command.ts`, plugin command registration `packages/plugin/src/{promise,effect}/command.ts` (IUMBTEMS registers `/grill`, `/es-approve` etc. this way) | **Plugin-only** for a `/factory` command + hidden seats; deeper OS-level isolation (bwrap read-only seats — `IUMBTEMS/README.md` limits) would be **core/CLI patch** territory (`packages/core/src/shell/`, `pty`) |
| 8 | scraper-swarm scoped keys, rate limits, audit as the fork's default web egress policy | Ship a Prime default `opencode.json` fragment + plugin | Config surface `packages/core/src/config/normalize.ts` (mcp block + plugin array); the fork can bundle a default plugin list (cf. `packages/core/src/plugin/mcp-codemode-defaults.ts` precedent for bundled defaults) | **Config/plugin-only**; shared rate-limit bucket across replicas = gateway-side change (`_check_rate_limit` seam, §2.3) |
| 9 | swarmd engine fleet & panel console as Prime "intelligence" dashboards | Out-of-process; surface via TUI panel or external browser | Panels: `packages/plugin/src/tui` + storybook (see #6); data via panel-api HTTP (admin-scoped) or gateway `/mcp` | **Plugin-only** (external service integration) |

Cross-cutting note: IUMBTEMS's own capability matrix (`IUMBTEMS/docs/CAPABILITIES.md`) already enumerates the rows the fork would inherit (agents, skills, tools, permissions, hooks, question, subagents, compaction, websearch, lsp, panels, research, brainstorm, harvest, design, claims, audit, scout — all ENFORCED for opencode with test pointers). Its "Known enforcement limits" (`IUMBTEMS/README.md`) should be treated as the honest threat-model appendix for any Prime marketing.

---

## 4. Maturity assessment

### 4.1 IUMBTEMS

| Dimension | Evidence | Assessment |
| --- | --- | --- |
| Tests | 40 `*.test.ts` files under `packages/` (26 in `packages/core/test/` incl. `factory.test.ts`, `gates.test.ts`, `audit-trust.test.ts`, `claims.test.ts`, `policy.test.ts`; 13 in `packages/opencode/test/`; `packages/cli/test/cli.test.ts`; `packages/testkit/test/`); snapshot tests (`test/__snapshots__/`); eval harness `scripts/evals.ts` + 6 eval cases with recorded results (`evals/results/2026-09-27T…/aggregate-result.json`, `report.html`) | Strong for its size; evals opt-in and step-capped (README limits) |
| CI | `.github/workflows/ci.yml` (check job: `bun run check` = lint + deps:check + typecheck + test; docs drift `bun run docs:check`; pack job: `scripts/pack-smoke.sh` build/pack/install and run under Node 22 + Bun; actions SHA-pinned, `permissions: contents: read`); `nightly.yml`, `publish.yml` (nightly `scripts/v2-head.sh` runs the real-host suite against OpenCode v2 HEAD) | Mature CI hygiene |
| Docs | `README.md`, `docs/{CAPABILITIES,CONFIG,DOGFOOD,SCHEMAS}.md`, 50+ generated schemas, `CHANGELOG.md`, per-module headers with rationale; `audit.md` is a standing adversarial-audit brief | Good; generated-doc drift is CI-enforced |
| LOC | ~32.7 k TS/TSX in `packages/` (core 25.7 k incl. tests, opencode 4.3 k, cli 2.3 k, testkit 0.3 k) | Compact |
| Production-ready | Published npm `@heretek-ai/epistemic-swarm@1.0.5` (registry); OpenCode path ENFORCED with tests; approvals/gates/dual-QA all live code with on-disk dogfood (`.legacy-state/` runs with qa-tiebreaks/escalations) | **The OpenCode-v2 path is production-usable today** |
| Aspirational | Claude Code/Pi/Antigravity adapters (all UNSUPPORTED rows, "ships in 1.1–1.3"); README status "1.0 in development"; enforcement limits (bwrap fallback, advisory MCP identity, control-file baseline scope) | Multi-harness story and some integrity guarantees are roadmap/best-effort — label them as such |

### 4.2 scraper-swarm

| Dimension | Evidence | Assessment |
| --- | --- | --- |
| Tests | 11 test files: `apps/gateway/tests/test_gateway.py` (scopes/revoke/expiry/rate per `docs/opencode-integration.md` §5), `apps/panel-api/tests/{test_agent_keys,test_api_flow,test_core_foundation,test_ssrf_guard,test_status_drift}.py`, `apps/swarmd/tests/{test_live_docker,test_reconcile,test_render_policy}.py`, `tests/security/test_ssrf.py`, `packages/opencode-plugin/src/index.test.ts` (vitest) | Decent coverage of auth/guard logic; live-engine tests exist but history shows flakiness (QA reports: "engines down at QA time", `.factory/scraper-swarm-factory/state.json`) |
| CI | `.github/workflows/ci.yml`: `uv sync` + `pytest apps/ -v`, pnpm frontend build + plugin tests (jobs `python-test`, `frontend-and-plugin-test`) | Present, less hardened than IUMBTEMS's (unpinned actions) |
| Docs | `docs/DEPLOYMENT.md` (production operator guide: requirements, bootstrap, zero-trust ingress, MFA wizard), `docs/opencode-integration.md` (detailed, self-citing, includes residual-risk disclosure), `docs/operator-reconcile.md`, `docs/spikes/m0-findings.md` (8 plan-changing findings with VERIFIED/UNVERIFIED discipline) | Strong operator docs; **no root README**; naming inconsistencies (`swarm_fetch`/`swarm_search`) |
| LOC | ~11.4 k (panel-api 3.9 k, panel-web 3.7 k, swarmd 2.0 k, gateway 1.5 k, plugin 0.3 k) | Small, focused |
| Production-ready | Gateway auth/scope/rate/SSRF/audit code + tests; installer + compose + tunnels; AES-GCM vault; drift-repair reconciler; factory-built with QA evidence | **Gateway + panel + swarmd are deployable self-hosted products**; rate limiting is single-process (shared bucket = future Valkey seam), `stealth_scrape`/`deep_research` depend on third-party engine images (licensing caveats: CloakBrowser license key, Firecrawl unauthenticated default — `docs/spikes/m0-findings.md` rows 4-5) |
| Aspirational | "live intelligence" / "distributed scraping" as named products (**NOT FOUND**); multi-replica budgets; plugin hook-surface compatibility with OpenCode v2 `@opencode/plugin` (self-declared context, no dependency) | Marketing terms and the plugin/v2 binding need work before Prime integration |

### 4.3 heretek-swarm & Heretek-Harness (relevance, briefly)

- **`/home/john/Projects/heretek-swarm`** — "Heretek Swarm — The Collective" v0.2.0 (Python 3.11+, `README.md:1-4`): a 23-agent autonomous multi-agent society (`PRIME_DIRECTIVE.md`) with Postgres/Redis/Qdrant/NATS/mem0/React dashboard (`docker-compose.yml`, `swarm-dashboard/`), its own `opencode.jsonc` configuring OpenCode with remote MCP (context7) + local MCP (`openwork-ui-mcp`) and a default agent "openwork" (`opencode.jsonc:1-25`). GitHub `https://github.com/Heretek-AI/heretek-swarm` exists (public, Python, updated 2026-08-13 — repo_view), i.e. **older than IUMBTEMS/scraper-swarm** (2026-10). Relevance: a prior-generation orchestration/consensus experiment and a live example of configuring OpenCode via `mcp`/`plugin` blocks; not part of the factory stack (no `es_*`/factory coupling found in a top-level scan).
- **`/home/john/Projects/Heretek-Harness`** — **empty directory locally** (zero entries). GitHub `https://github.com/Heretek-AI/Heretek-Harness` exists (public, no description, 0 stars, updated 2026-08-16 — repo_view). Nothing to analyze on disk; relevance NOT FOUND beyond the name. (Distinct from the archived `Heretek-AI/heretek-claude-harness` — repo_view search results, "Archived: true".)

---

## 5. Open questions / UNVERIFIED items

1. **scraper-swarm plugin ↔ fork plugin-host compatibility** — hook names `tool.execute.before`/`command.executed` and a hand-rolled `PluginContext` vs the fork's `@opencode/plugin` promise/effect API (`packages/plugin/src/promise/plugin.ts`, `packages/core/src/plugin/hooks.ts`). Needs one runtime smoke (out of scope here: no runs per mission rules). UNVERIFIED.
2. **`swarm_fetch`/`swarm_search` naming** — referenced in `scraper-swarm/docs/DEPLOYMENT.md:114`, plugin error text and tests, but absent from the gateway tool registry (`web_search`, `fetch_page`, `stealth_scrape`, `deep_research`). Either the docs are stale or a tool rename is planned; NOT FOUND as code.
3. **"323+ repos", pricing and model names in the mission brief** — out of this track's scope; `xd://github search_repos org:Heretek-AI` returned 50 results in one page (the org clearly has ≥50 repos; GrindrPlus-ReMorphe, shadchamp-bot, openclaw-legacy are private). Not verified further here.
4. **IUMBTEMS "1.0 in development" vs npm 1.0.5** — README and registry disagree; registry observed 2026-10-07.

---

## Sources

Local primary sources (paths as cited inline):
- `/home/john/Projects/IUMBTEMS/README.md`, `audit.md`, `docs/CAPABILITIES.md`, `package.json`
- `/home/john/Projects/IUMBTEMS/packages/core/src/factory/{machine,state}.ts`, `packages/core/src/ops/tools.ts`, `packages/core/src/agents/registry.ts`, `packages/core/src/approval/{record,keystore}.ts`, `packages/core/src/claims/witness.ts`, `packages/core/src/research/{auditor,cache}.ts`
- `/home/john/Projects/IUMBTEMS/packages/opencode/src/{server,tools,tui,rpc,rpc-def,panels,research}.ts(x)`, `packages/opencode/package.json`, `packages/opencode/test/*`
- `/home/john/Projects/IUMBTEMS/packages/cli/src/{human,mcp}.ts`; `schemas/`; `evals/`; `.github/workflows/{ci,nightly,publish}.yml`; `.legacy-state/roadmap/**/qa-tiebreak-*.md`
- `/home/john/Projects/scraper-swarm/docs/{DEPLOYMENT,opencode-integration,operator-reconcile}.md`, `docs/spikes/m0-findings.md`
- `/home/john/Projects/scraper-swarm/apps/gateway/src/gateway/server.py`, `apps/panel-api/src/panel_api/{main,vault}.py`, `apps/panel-api/src/panel_api/routers/agents.py`, `apps/swarmd/src/swarmd/{server,reconciler}.py`, `catalog/services/*.yaml`, `packages/opencode-plugin/src/{index.ts,index.test.ts}`, `packages/opencode-plugin/package.json`, `.factory/scraper-swarm-factory/state.json`, `.roadmap/*/GOAL.md`, `.github/workflows/ci.yml`
- `/home/john/Projects/opencode/AGENTS.md`, `packages/plugin/src/{promise,effect,tui}/`, `packages/core/src/{tool.ts,tool/runtime.ts,tool/mcp.ts,mcp/client.ts,mcp/index.ts,permission.ts,permission/{saved,sql}.ts,plugin/hooks.ts,plugin/module.ts,config/normalize.ts}`, `packages/tui/src/feature-plugins/system/storybook/`
- `/home/john/Projects/heretek-swarm/{README.md,PRIME_DIRECTIVE.md,opencode.jsonc}`

External (checked 2026-10-07):
- https://github.com/Heretek-AI/IUMBTEMS (repo_view: public, TypeScript, default branch `main`, updated 2026-10-07)
- https://github.com/Heretek-AI/scraper-swarm (repo_view: public, Python, default branch `main`, updated 2026-10-07)
- https://github.com/Heretek-AI/opencode (repo_view: public fork of anomalyco/opencode, default branch `dev`, updated 2026-10-07)
- https://github.com/Heretek-AI/heretek-swarm (repo_view: public, Python, updated 2026-08-13)
- https://github.com/Heretek-AI/Heretek-Harness (repo_view: public, empty description, updated 2026-08-16)
- https://registry.npmjs.org/@heretek-ai/epistemic-swarm (`dist-tags.latest: "1.0.5"`)
- GitHub org listing: `xd://github search_repos "org:Heretek-AI"` (50 results, 2026-10-07)
