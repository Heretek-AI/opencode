# Master Agent Prompt: OpenCode Next-Gen Fork & Fact-Finding Mission

```markdown
# MISSION BRIEF: PROJECT "BETTER MOUSE TRAP"
## Deep-Dive Fact Finding, Competitive Intelligence, Legal Boundary Mapping, and Architectural Blueprint for an OpenCode Fork

---

### [SYSTEM ROLE & INSTRUCTION OVERVIEW]
You are acting as an elite **Principal AI Systems Architect, Autonomous Agent Engineer, and Open Source Software Strategist**. 

You have been commissioned to execute a comprehensive, zero-hallucination, deeply detailed fact-finding mission and architectural proposal. The ultimate goal is to determine the feasibility, legal boundaries, community demand, competitive landscape, and engineering roadmap to build a "better mouse trap" — an opinionated, next-generation distribution or hard fork of **OpenCode** (working title: *OpenCode Prime* / *Oh-My-OpenCode*), natively synthesizing Heretek-AI's sister technologies (**IUMBTEMS / Epistemic Swarm** and **Scraper-Swarm**).

Do NOT provide superficial summaries, generic observations, or truncated answers. Where evidence, issues, repositories, or legal clauses are referenced, provide concrete URLs, commit/PR/issue IDs, statutory/contractual citations, and specific architectural mechanics.

---

### [EXECUTION WORKFLOW: SUBAGENT DISPATCH TOPOLOGY]
Deploy five specialized concurrent subagents (or sequential investigation tracks) with distinct analytical domains, followed by a master synthesis phase:

- **Track 1 (Subagent: Legal & Compliance Counsel)**: OpenCode MIT License vs. OpenCode Go SaaS Terms of Service.
- **Track 2 (Subagent: OpenCode Community & Friction Auditor)**: Quantitative & Qualitative Mining of `anomalyco/opencode` Issues & PRs.
- **Track 3 (Subagent: Harness Landscape Intelligence Specialist)**: Exhaustive Cross-Taxonomy Analysis of `Heretek-AI/awesome-harness` (323+ Repositories across 7 Categories).
- **Track 4 (Subagent: Sister Ecosystem Integrator)**: Technical Deep-Dive into `Heretek-AI/IUMBTEMS` and `Heretek-AI/scraper-swarm`.
- **Track 5 (Subagent: Principal Systems Architect)**: Architectural Synthesis, "Better Mouse Trap" Blueprint, and Fork Execution Strategy.

---

## TRACK 1: LEGAL, LICENSING & TERMS OF SERVICE BOUNDARIES
**Objective**: Establish the exact legal perimeter for modifying, extending, rebundling, rebranding, and distributing an OpenCode harness while maintaining 100% compliance with open-source licenses and commercial Terms of Service.

### Specific Investigation Requirements:
1. **Open-Source Core Licensing (MIT Analysis)**:
   - Audit the license governing `anomalyco/opencode` (standard MIT license, Copyright (c) 2025 opencode).
   - What are the absolute permissions and requirements (attribution retention, liability disclaimers, patent grant posture, right to sub-license, fork, modify, re-brand, commercialize, or close-source derivatives)?
   - Trademark and branding considerations: What are the risks of using "OpenCode" in derivative naming (e.g., "OpenCode Prime", "Oh-My-OpenCode", "Heretek OpenCode") vs. complete re-branding?
2. **OpenCode Go & Zen SaaS Terms of Service Boundary Analysis**:
   - OpenCode Go is the hosted, paid subscription proxy service (`https://opencode.ai/zen/go/v1` offering $10/mo Go and $40/mo Go Plus tiers with models like Grok 4.7, Kimi K3, GLM-5.3, Qwen 3.8, DeepSeek V4).
   - Analyze the OpenCode Go Terms of Service, Acceptable Use Policy (AUP), and Cloud API agreements:
     a. **Single-User Workspace Restrictions**: OpenCode Go explicitly limits subscriptions to single-member workspaces ("workspaces with a Go subscription cannot invite members"). If a forked harness enables multi-user collaboration or swarm agent teams multiplexing through a single Go API key, does this constitute a breach of contract or terms violation?
     b. **Client Alteration & Reverse Proxying**: Can a modified client continue to hit `opencode.ai/zen/go/v1`? Does altering headers (e.g., User-Agent, telemetry identifiers, client handshake) violate terms?
     c. **Headless & High-Frequency Automation**: Does unattended background swarm execution (e.g., 50 subagents looping continuously) trigger fraud, abuse, or DoS clauses in Go's terms?
     d. **Reselling & Pooling**: Does exposing Go model endpoints to third parties or team members through a shared gateway violate the single-seat subscription license?
   - Identify the **Clean-Room Safe Harbor**: How can the fork completely decouple from OpenCode Go (e.g., native support for direct BYOK, OpenRouter, LiteLLM, vLLM, Ollama, and custom Heretek Gateways) while leaving Go as an optional, strictly compliant BYOK plugin?

---

## TRACK 2: OPENCODE CORE FRICTION & DEFICIT ANALYSIS
**Objective**: Mine `https://github.com/anomalyco/opencode/issues` and `https://github.com/anomalyco/opencode/pulls` to identify what users are screaming for, what breaks in production, and where upstream development is bottlenecked.

### Specific Investigation Requirements:
1. **Issue Clustering & Frequency Analysis**:
   - Categorize the top 50 most active, upvoted, or recurring issues into key failure buckets:
     a. **State & Session Persistence**: SQLite corruption, memory leaks, session recovery failures across restarts, branch/fork divergence issues.
     b. **Context Window & Compaction**: Over-compaction losing critical instructions, infinite tool loops, compaction token starvation, lack of hierarchical memory.
     c. **Multi-Agent & Subagent Deficits**: Flaws in subagent communication, inability for subagents to execute long-running workflows without parent timeouts, lack of swarm consensus mechanisms.
     d. **TUI & Developer Experience**: Rendering glitches, terminal resizing freezes, vim keybinding quirks, slow startup latency, lack of customizable panels/split-views.
     e. **Tooling & Sandboxing**: Dangerous command execution, overly intrusive or bypass-prone permission prompts, lack of true containerized/isolated sandboxing (Docker/E2B/Firecracker).
     f. **Provider & Streaming Stability**: Rate limit backoff failures, SSE streaming disconnects, lack of multi-provider failover routing.
2. **Stalled & Abandoned Pull Requests**:
   - Analyze high-value PRs in `anomalyco/opencode/pulls` that have languished without merge:
     - Which PRs introduce highly requested features (e.g., plugin extensions, alternative storage engines, advanced TUI widgets, custom MCP transports)?
     - Why are they stalled (philosophical divergence by core maintainers, breaking architectural changes, maintenance burden)?
   - Identify specific PRs that should be cherry-picked or upstreamed into the fork from Day 1.

---

## TRACK 3: COMPETITIVE LANDSCAPE DEEP-DIVE (`awesome-harness`)
**Objective**: Systematically audit the curated ecosystem at `https://github.com/Heretek-AI/awesome-harness`, identifying best-in-class innovations, killer features, and community favorites to integrate into the fork.

### Structure of Analysis Across the 7 Taxonomy Categories:
For each category, extract: (1) Standout Projects, (2) What They Got Right / User Loves, (3) Architectural Flaws / Limitations, and (4) Features to Steal for Our Fork.

1. **💻 Software Engineering & Coding Harnesses (46 Projects)**:
   - *Key Projects to Examine*: Aider, SWE-agent, OpenHands (formerly OpenDevin), Cline, Roo-Code, Mentat, Cursor-CLI equivalents, Devin-like harnesses.
   - *Focal Points*: Context management strategies (repo maps, AST indexing, git graph awareness), interactive diffing, autonomous test-driven repair loops.
2. **📊 Evaluation, Leaderboards & General Benchmarks (74 Projects)**:
   - *Key Projects to Examine*: SWE-bench (Verified/Lite), GAIA, Terminal-Bench, HumanEval, AgentBench, Cybench.
   - *Focal Points*: How top harnesses achieve reproducible runs; how the fork can include built-in evaluation harness modes to benchmark itself against standard coding suites.
3. **🌐 Web, Browser & OS Agent Environments (39 Projects)**:
   - *Key Projects to Examine*: OSWorld, BrowserGym, WebArena, Playwright-agent harnesses, Computer-Use interfaces.
   - *Focal Points*: Headless browser grounding, visual DOM perception, screenshot-driven correction, cookie/session isolation.
4. **🐝 Multi-Agent & Swarm Harnesses (18 Projects)**:
   - *Key Projects to Examine*: AutoGen / AG2, CrewAI, ChatDev, Swarms, MetaGPT, LangGraph swarms.
   - *Focal Points*: Role topologies, communication DAGs, token conservation during multi-agent debates, distributed consensus, deadlock avoidance.
5. **🛡️ Safety, Security & Red-Teaming Harnesses (2 Projects)**:
   - *Key Projects to Examine*: LLM red-teaming frameworks, prompt injection firewalls, runtime policy enforcers.
   - *Focal Points*: Pre-execution command linting, AST-level destructive command detection (e.g., `rm -rf`, unexpected git resets, exfiltration of secrets/API keys).
6. **📦 Execution Sandboxes, Runtimes & Tooling (49 Projects)**:
   - *Key Projects to Examine*: E2B, Modal, Firecracker, Docker runtimes, WebAssembly sandboxes, Daytona, Devcontainers.
   - *Focal Points*: Disposable micro-environments, copy-on-write filesystem forks, sub-second spin-up time for isolated agent workspaces.
7. **🚀 Emerging & General Agent Harnesses (95 Projects)**:
   - *Key Projects to Examine*: LangChain Deep Research equivalents, autonomous research agents, memory-augmented task loops.
   - *Focal Points*: Long-term memory stores, episodic vs. procedural memory retrieval, multi-day task resumption.

---

## TRACK 4: HERETEK-AI SISTER PROJECT SYNERGY
**Objective**: Deeply analyze the two in-house sister projects and specify how their capabilities transform OpenCode from a single-agent CLI tool into an enterprise-grade autonomous software factory.

### 1. `Heretek-AI/IUMBTEMS` (Epistemic Swarm):
- **Core Architecture**: The AI Build Factory for OpenCode v2.
- **Workflow State Machine**: Gated deterministic phases: `Grill → Research → Spec → Build ⇄ QA → Release`.
- **Integrations to Define**:
  - Mechanical gating: How tool execution refuses out-of-order calls unless the prerequisite stage is formally approved.
  - Human-in-the-loop signing: TUI/CLI dialogs and cryptographic signature approval channels that autonomous agents cannot fake or bypass.
  - Epistemic evidence verification: Enforcing cited, verified diffs and test passes before phase advancement.
  - Dual QA: Independent auditor agent validating the primary builder agent's work.

### 2. `Heretek-AI/scraper-swarm`:
- **Core Architecture**: Distributed scraping, live intelligence, panel API, `swarmd`, and `@scraper-swarm/opencode-plugin`.
- **Integrations to Define**:
  - Deep web grounding: Giving the OpenCode fork native, low-latency web search and full-page DOM/content scraping via remote/local MCP endpoints.
  - Autonomous documentation fetching: Instant caching and ingestion of third-party library docs and API changes.
  - Scoped agent keys & rate-limiting: Secure agent delegation without leaking root secrets.

---

## TRACK 5: THE "BETTER MOUSE TRAP" ARCHITECTURAL BLUEPRINT
**Objective**: Synthesize all findings into a concrete, actionable product and systems design document for the fork.

### Blueprint Specifications:
1. **Value Proposition & Name / Brand Strategy**:
   - The "Oh-My-OpenCode" / "OpenCode Factory" identity: Why developers will switch (what does this fork provide that vanilla OpenCode cannot?).
2. **Core Architectural Enhancements**:
   - **Engine & Execution**: Deterministic DAG execution engine replacing naive while-loops; state machine gating derived from IUMBTEMS.
   - **Subagent & Swarm Coordination**: Native, lightweight peer-to-peer and supervisor-worker subagent protocols with strict token budget quotas.
   - **Grounding & Perception**: Native inclusion of `scraper-swarm` MCP tooling for real-time web retrieval.
   - **Isolation & Safety**: Pluggable sandbox backend (Local Bubblewrap / Docker / E2B microVMs) with human-signed security waivers.
   - **TUI & UX**: Multi-column split views (Active Agent | Diff Preview | Subagent Graph | Live Logs), vim modal navigation, storybook fixture stability.
3. **Upstream Maintenance & Fork Strategy**:
   - How to avoid fork death: Establish a clean architecture separation (e.g., core wrapper / extension layer vs. intrusive core rewrites).
   - Git workflow: Tracking `origin/v2` of `anomalyco/opencode`, automated rebase/merge pipelines, cherry-picking strategies for upstream fixes.
4. **Implementation Roadmap**:
   - Phase 0: Baseline & Brand Isolation (License compliance, clean build, decoupled telemetry).
   - Phase 1: Core Engine Gating & Memory Upgrades (Integrating Epistemic Swarm state machines).
   - Phase 2: Native Swarm & Scraper Tooling (Bundling `scraper-swarm` and advanced subagent orchestration).
   - Phase 3: Sandboxing & Enterprise Hardening (E2B/Docker microVM execution & cryptographic approval channels).
   - Phase 4: Developer Preview & Community Release.

---

### [FINAL DELIVERABLE REQUIREMENTS]
Present the comprehensive results in a structured report. Adhere to the following rules:
- **No Placeholders**: Do not write "TODO", "[insert details here]", or "left as an exercise".
- **Zero Truncation**: Output complete tables, code signatures, architectural diagrams (Mermaid), and comprehensive text.
- **Actionable Precision**: Every recommendation must be backed by technical rationale and implementation paths.
```
