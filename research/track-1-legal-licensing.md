# Track 1 — Legal, Licensing & Terms-of-Service Boundaries

Research date: 2026-10-07. Every claim is cited to a primary source (file path + line, URL, or API response observed on the research date). Items that could not be verified are labeled `UNVERIFIED` or `NOT FOUND` with the search trail. Nothing here is legal advice; the "assessment" paragraphs are engineering-risk judgments, not legal opinions.

---

## 1. MIT core licensing analysis

Primary source: `/home/john/Projects/opencode/LICENSE` (plain-text MIT License, "Copyright (c) 2025 opencode", LICENSE:1-3).

### 1.1 Absolute permissions (mapped to clause text)

The single grant clause (LICENSE:5-8) reads:

> "Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so…"

| Permission | Clause text | Notes |
|---|---|---|
| Fork / use | "to deal in the Software without restriction… rights to use, copy" (LICENSE:5-7) | No field-of-use, geography, or purpose limits |
| Modify / rebrand code | "modify, merge" (LICENSE:7) | Rebranding code, configs, UI strings is a "modification" — permitted by the license (trademark law is separate, §1.3) |
| Publish / distribute | "publish, distribute" (LICENSE:7) | Source or binaries |
| Sublicense | "sublicense" (LICENSE:7-8) | Express — the fork may place modified/derived code under different terms |
| Commercialize / sell | "and/or sell copies of the Software" (LICENSE:8) | Express — paid distribution allowed |
| Close-source derivatives | follows from "sublicense" + "publish, distribute" with no copyleft requirement; the only retention duty is §1.2(1) | A proprietary fork is permitted as long as the copyright/permission notice is kept in the distributed copies |

### 1.2 The two conditions

1. **Attribution retention** (LICENSE:9-11):
   > "The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software."
   This is the only affirmative obligation. "Copyright (c) 2025 opencode" (LICENSE:3) + the permission paragraph must travel with every copy (source or binary). It does **not** require naming the product "OpenCode" — retaining the notice text in a LICENSE/NOTICE file satisfies it.
2. **Warranty/liability disclaimer** (LICENSE:13-22):
   > "THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY…"
   Precision note: legally this is a disclaimer that rides with the grant, not a behavioral condition; practically it is treated as the second "term" — it may not be stripped from the license text, and it means the upstream gives **no warranties and no indemnity** to the fork. The fork likewise takes the code "AS IS" from upstream and should disclaim in its own LICENSE the same way.

### 1.3 Patent posture

- **MIT contains NO explicit patent grant.** `grep -ci patent LICENSE` → `0` (verified 2026-10-07). There is no express patent license, no patent-retaliation/termination clause, and no defensive-termination provision (contrast Apache-2.0 §3). At most, U.S. case law may imply a narrow license for patents "necessarily infringed" by exercising the copyright grant — that is general doctrine, not a repository fact (`[UNVERIFIED]` as applied to any specific patent).
- **No CLA / DCO / patent policy exists in the repo or upstream project.** Search trail:
  - `CONTRIBUTING.md`, `AGENTS.md`, `SECURITY.md`, `README.md`: case-insensitive grep for `CLA|Contributor License|patent|trademark|DCO|Signed-off` → **0 matches**. CONTRIBUTING.md says only which PRs will be accepted and links issues to `github.com/anomalyco/opencode` (CONTRIBUTING.md:12).
  - `.github/` (CODEOWNERS, ISSUE_TEMPLATE, TEAM_MEMBERS, pull_request_template.md, workflows/) and `github/` (the repo's CI action): grep for `CLA|patent|DCO|sign-off` → **0 matches**.
  - GitHub issue search `CLA contributor license agreement` in `anomalyco/opencode` → **0 results** (xd://github search_issues, 2026-10-07).
- **Practical risk:** every contribution was made under the bare MIT copyright license. A contributor (or their employer) could in principle assert patent claims against users of the code, with no express license and no defensive-termination deterrent. Risk is *low in practice* for a developer-tool codebase but is structurally unmitigated. Options: (a) accept the risk like most MIT projects; (b) place **new Heretek-authored code** under Apache-2.0 or a patent-pledged license (upstream MIT code cannot be relicensed, but its grants stack additively); (c) patent due-diligence on subsystems where a single employer contributed heavily before commercialization.

### 1.4 Trademark

- **MIT is silent on trademarks.** The LICENSE text contains no "trademark"/"trade dress" clause (grep across LICENSE/CONTRIBUTING/README/SECURITY → 0 matches). The MIT grant therefore does **not** include any trademark license; using "OpenCode" as a brand is governed by trademark law only.
- **Actual trademark situation — a US registration exists:**
  - USPTO serial **99379413**, word mark **"OPENCODE"** (standard character mark, drawing code 4), applicant **Anomaly Innovations Inc.** (San Francisco, CA; the ToS counterparty, §3.2), filed **2025-09-08**, International Classes **9** ("Downloadable software using large language models (LLMs) for computer software code generation") and **42** ("Software as a service (SAAS) services featuring software using artificial intelligence (AI) for autonomous coding"); first use anywhere 2025-06-19 (Cl. 9) / 2025-09-03 (Cl. 42) (Justia record: https://trademarks.justia.com/993/79/opencode-99379413.html, fetched 2026-10-07).
  - **Registered**: Trademarkia record (fetched 2026-10-07) states: "OPENCODE is a registered trademark (**Registration #8267298**) owned by Anomaly Innovations Inc. … filed on 08 Sep 2025 with serial number (#99379413) and **registered on 19 May 2026**", "Status: Live/Registered … Status as of | 19 May 2026 REGISTERED" (https://www.trademarkia.com/opencode-99379413). The Justia copy is stale — it still shows pre-registration status "630 - New Application - Record Initialized Not Assigned To Examiner" with only the 2025-09-08 filing events. Where they conflict, the Trademarkia "REGISTERED" status is the later record; **registration number 8267298 could not be independently confirmed against USPTO TSDR (requires JS/API key) — treat the registration number as `[UNVERIFIED-2ND-HAND]`, the application/serial/owner as verified.**
  - The Justia owner page lists two "OPENCODE" entries for Anomaly Innovations Inc. both filed 2025-09-08 (https://trademarks.justia.com/owners/anomaly-innovations-inc-6420092) — likely the word mark plus a logo variant; details of the second entry NOT retrievable (page returned JS shell).
- **No trademark policy/notice in the product.** No `TRADEMARK*` file exists anywhere in the repo (glob `**/*TRADEMARK*` → none, 2026-10-07). opencode.ai homepage contains no "trademark" or copyright-notice string (curl + grep → 0 matches). ToS contains only a generic IP-enforcement reference ("…a party's copyrights, trademarks, trade secrets, patents or other intellectual property rights", ToS "Small Claims Court; Infringement", `packages/console/app/src/routes/legal/terms-of-service/index.tsx:444`).
- **WIPO: NOT FOUND.** WIPO Global Brand Database (https://branddb.wipo.int) is a JS query interface that could not be scraped; web searches `WIPO global brand database "OPENCODE" trademark` and `"opencode" USPTO trademark -anomaly` surfaced no Madrid/foreign registration for OPENCODE (searched 2026-10-07 via web_search). Absence of a search hit is not proof of absence.
- **Third-party "opencode" naming in the wild (confusion risk, not necessarily infringement):**
  - `uspto.report/company/Opencode-Inc` shows a separate entity "Opencode Inc" with 1 trademark application (latest "OPEN…", per search-result snippet; full record blocked by Cloudflare JS challenge — `[UNVERIFIED]`).
  - `github.com/opencode-ai/opencode` is an unrelated **archived** Go-language AI coding agent (13,789 stars, default branch `main`; xd://github repo_view 2026-10-07). The name "opencode" is heavily overloaded in this ecosystem.
  - No open TTAB opposition against the OPENCODE mark was found; the TTAB document surfaced by search (TTAB 91299649) is *OpenAI OpCo, LLC v. applicant of SENSORGPT, Ser. 98/451,729* — unrelated (verified by pdftotext of https://ttabvue.uspto.gov/ttabvue/ttabvue-91299649-OPP-1.pdf, lines 365-395).

**Naming-risk assessment (engineering judgment, not legal advice):**

| Name | Risk | Rationale |
|---|---|---|
| "OpenCode Prime" | **HIGH** | Embeds the registered standard-character word mark in full, in the identical goods/classes (Cl. 9 software, Cl. 42 SaaS). Descriptive add-ons ("Prime") do not avoid likelihood of confusion for identical goods. |
| "Oh-My-OpenCode" | **HIGH** | Same incorporation of the mark in the product brand; the "oh-my-" prefix is a naming convention (cf. oh-my-zsh) that still uses OPENCODE as the source identifier. |
| "Heretek OpenCode" | **HIGH** | Composite mark containing the registered term for identical goods; a "house mark + third-party mark" composite is the classic infringement pattern. |
| Full rebrand ("Heretek Prime" etc.) + "a fork of OpenCode" in documentation only | **LOW** | Nominative/descriptive text use is materially lower risk than brand use; MIT attribution is satisfied by retaining the LICENSE text verbatim (§1.2), which does not require trademark use. |

Recommended regardless of naming: (a) keep "Copyright (c) 2025 opencode" + MIT permission text verbatim in LICENSE (LICENSE:9-11 obligates exactly this); (b) add an unaffiliation disclaimer ("not affiliated with, sponsored by, or endorsed by Anomaly Innovations Inc."); (c) do not reuse upstream logos (`services/www`/`packages/web` brand assets, e.g. `packages/web/src/docs/assets/logo-*.svg` via services/www public assets) or the opencode.ai domain/`opencode.ai` URLs in the fork's branding.

---

## 2. Upstream identity verification

**The true upstream of this codebase is `anomalyco/opencode` (Anomaly Innovations), formerly `sst/opencode`.** The brief's reference is correct; `Heretek-AI/opencode` is a GitHub fork of it.

Evidence:

1. **GitHub API fork parent (primary):** `GET api.github.com/repos/Heretek-AI/opencode` (2026-10-07) → `"fork": true`, `"parent": {"full_name": "anomalyco/opencode", "id": 975734319}`. The fork was **created 2026-10-07T13:52:52Z** (i.e., today — a fresh fork).
2. **Rename/transfer from sst (primary):** `GET api.github.com/repos/sst/opencode` (2026-10-07) → `"message": "Moved Permanently", "url": "https://api.github.com/repositories/975734319"` — the exact repository id now at `anomalyco/opencode`. `xd://github` repo_view of `sst/opencode` likewise resolves to `anomalyco/opencode`. Corroboration (secondary sources, `[SECONDARY]`): rebrand trackers date the org move to January 2026 (https://www.aipotluck.org/product/opencode — "renamed from sst/opencode to anomalyco/opencode and the old URL redirects. Verified 2026-08-13"; https://aiwiki.ai/wiki/opencode — Dax Raad post of 2 Jan 2026 about "rebranding everything under" the name "anomaly").
3. **README badges/links (local file):** `README.md:14` build badge → `github.com/anomalyco/opencode/actions/workflows/publish.yml?branch=dev`; `README.md:56` `brew install anomalyco/tap/opencode`; `README.md:69` releases at `github.com/anomalyco/opencode/releases`.
4. **git log lineage (local repo):** top commit authors include Dax Raad `<d@ironbay.co>` (2,451 commits), Kit Langton, Aiden Cline, and the historical committer identity `opencode <opencode@sst.dev>` (575 commits) — proof the history descends from the sst-era project. Commit subjects carry upstream PR numbers; local HEAD `119cb15d1d "refactor(app): remove configurable default server (#53731)"` matches **anomalyco/opencode PR #53731** (merged 2026-10-07, xd://github search_prs).
5. **CONTRIBUTING.md** links all issue labels to `https://github.com/anomalyco/opencode/issues?...` (CONTRIBUTING.md:12) and references `anomalyco/models.dev` for provider additions (CONTRIBUTING.md:20-22).
6. **Corporate identity:** the ToS binds users to "**ANOMALY INNOVATIONS, INC.**" ("OpenCode") (terms-of-service/index.tsx:29); contact `help@anoma.ly` (index.tsx:25); security trust center `trust.opencode.ai` is titled "Anomaly Innovations, Inc - Security Trust Center" (search result, 2026-10-07).

**Default-branch clarification (the brief's "default branch v2" is inaccurate):**

- `anomalyco/opencode` default branch is **`dev`** (GitHub API `default_branch`, 2026-10-07; README badge `branch=dev`).
- `Heretek-AI/opencode` default branch is also **`dev`** (GitHub API) — it mirrors upstream `dev` (head `ecc4916b5a`). The project's working branch is **`v2`** (GitHub API `branches/v2` head `119cb15d1d…` = the local checkout's HEAD). The local clone is single-branch: `remote.origin.fetch = +refs/heads/v2:refs/remotes/origin/v2`.
- Do not confuse with `opencode-ai/opencode` — a different, archived Go project (§1.4).

---

## 3. OpenCode Go / Zen SaaS ToS boundary

### 3.1 What actually exists (brief claims verified)

| Brief claim | Verdict | Primary evidence |
|---|---|---|
| "opencode.ai/zen (product)" | **VERIFIED** | `https://opencode.ai/zen` returns HTTP 200 (2026-10-07); ToS names it the "Paid Services page" (terms-of-service/index.tsx:274-277); docs `packages/web/src/content/docs/zen.mdx:10,40`: "OpenCode Zen is a list of tested and verified models… Zen is an AI gateway that gives you access to these models" |
| "$10/mo Go" | **VERIFIED** | `services/www/src/docs/content/console/go.mdx:10`: `| **Go** | **$10/month** |`; older doc `packages/web/src/content/docs/go.mdx:10` ("low cost **$10/month subscription**") |
| "$40/mo Go Plus" | **VERIFIED** | `services/www/src/docs/content/console/go.mdx:11`: `| **Go Plus** | **$40/month** |` |
| Endpoint `https://opencode.ai/zen/go/v1` | **VERIFIED** | Docs endpoints table (`console/go.mdx:391-420`: `https://opencode.ai/zen/go/v1/chat/completions|messages|responses`); server code `packages/console/function/src/log-processor.ts:17-20` whitelists `/zen/go/v1/chat/completions`, `/zen/go/v1/messages`, `/zen/go/v1/responses`, `/zen/go/v1/models/`; catalog entry `opencode-go` with `api: "https://opencode.ai/zen/go/v1"` in `packages/core/src/models-dev/snapshot.txt`; live `GET https://opencode.ai/zen/go/v1/models` → HTTP 200 (2026-10-07) |
| Model "Grok 4.7" | **VERIFIED** | `console/go.mdx:37` roster ("**Grok 4.7**"), pricing tables (Grok 4.7 ≤/>200K rows) and endpoints (`grok-4.7` → `/zen/go/v1/responses`) |
| Model "Kimi K3" | **VERIFIED** | `console/go.mdx:44` roster; both pricing plans; `kimi-k3` → `/zen/go/v1/chat/completions` |
| Model "GLM-5.3" | **VERIFIED** | `console/go.mdx:42-43` roster ("**GLM-5.3-Flash**, **GLM-5.3**"); pricing + endpoints (`glm-5.3`) |
| Model "Qwen 3.8" | **VERIFIED (as "Qwen3.8 Max" / "Qwen3.8 Flash")** | `console/go.mdx:54-55` roster; pricing; endpoints (`qwen3.8-max`, `qwen3.8-flash`) |
| Model "DeepSeek V4" | **VERIFIED (as DeepSeek V4.1 Flash / V4 Pro / V4 Flash / V4 Flash Vision Exp)** | `console/go.mdx:56-59` roster; pricing (peak/off-peak rows); endpoints |

Notes: (a) the brief's model list matches the **current** roster at `services/www/src/docs/content/console/go.mdx`; a **stale second copy** of the Go docs at `packages/web/src/content/docs/go.mdx:46-63` lists an older roster (Grok 4.5, GLM-5.2/5.1, GPT 5.6 Luna, MiMo-V2.5…) — both exist in the repo and should be cited by path. (b) Full current roster also includes GPT 6 Luna, GPT 5.6 Luna, LongCat-2.0, LongCat 2.5 Preview Free, MiMo-V2.6-Flash/Pro, MiniMax M3/M2.7, Muse Spark 1.2/1.3 Contributor, Qwen3.7 Plus, Hy3/Hy4 preview, Space Bunny (console/go.mdx:36-65). (c) Pricing mechanics: each model has a monthly included-usage cap ($15/$30/$60 on Go; $60-$240 on Go Plus), with 5-hour = 20%, weekly = 50%, monthly = 100% windows (console/go.mdx:113-119). (d) "No lock-in" is an explicit product goal ("Have **no lock-in** by allowing you to use any other provider with OpenCode as well", console/go.mdx:481).

### 3.2 Terms of Service / AUP / Cloud API agreement

**Terms of Service: FOUND and read.** Source of truth in-repo and live:

- Served at **https://opencode.ai/legal/terms-of-service** (HTTP 200, 2026-10-07; live HTML contains "Effective date: Aug 15, 2026", "ANOMALY INNOVATIONS, INC.", and the "own internal use" clause — matches repo source exactly).
- Page source: `packages/console/app/src/routes/legal/terms-of-service/index.tsx` (523 lines). Title "Terms of Use"; "Effective date: Aug 15, 2026" (index.tsx:22); binding contract with "ANOMALY INNOVATIONS, INC." ("OpenCode","we","us") (index.tsx:29).
- **Critical carve-out for the fork** (index.tsx:48-51):
  > "For clarity, our open source software that is not provided to you on a hosted basis is subject to the open source license and terms set forth on the applicable repository where you access such open source software, and such license and terms will exclusively govern your use of such open source software."
  I.e., the ToS governs the **hosted Services** (website, Zen/Go inference, hosted offering — index.tsx:26-28), not the MIT-licensed repo code.
- URLs tried: `https://opencode.ai/terms` → **404**; `https://opencode.ai/legal` → **404**; `https://opencode.ai/legal/terms-of-service` → 200 (2026-10-07). A privacy policy exists at `/legal/privacy-policy` (referenced at index.tsx:32; `packages/console/app/src/routes/legal/privacy-policy/`).

**Acceptable Use Policy: NOT FOUND.** Search trail (2026-10-07): grep `acceptable use|Acceptable Use|AUP|abuse@` (case-insensitive) across `packages/console/app/src` and English `packages/web/src/content/docs` → no AUP document; `packages/console/app/src/routes/legal/` contains only `terms-of-service/` and `privacy-policy/` directories; no `opencode.ai/usage-policy`-style link appears in the ToS (its "Additional Terms" clause, index.tsx:33-36, is generic and no Go/Cloud-specific additional terms were found in repo or via web search `opencode.ai acceptable use policy Go Zen cloud API agreement`).

**Separate "Cloud API agreement": NOT FOUND** (same search trail). Zen/Go are covered by the ToS "Paid Services" section: "Certain of our Services, including Zen, may be subject to payments… Please see our Paid Services page https://opencode.ai/zen… any payment terms presented to you in the process of using or signing up for a Paid Service are deemed part of these Terms" (index.tsx:271-277); billing via Stripe (index.tsx:283-286).

### 3.3 Clause mapping for the four boundary questions

All quotes from `packages/console/app/src/routes/legal/terms-of-service/index.tsx` (the text served at opencode.ai/legal/terms-of-service). The restrictions list (index.tsx:134-196) is an ordered list labeled (a)–(m) in source (`style="list-style-type: lower-alpha;"`, index.tsx:142).

**(a) Single-user / single-seat workspace restrictions**

- ToS (hosted Services generally): "You will only use the Services for your own internal use, and not on behalf of or for the benefit of any third party, and only in a manner that complies with all laws that apply to you." (index.tsx:131-132, "What are the basics of using OpenCode?")
- Product-level restriction (Go-specific, in docs): "**Go and Go Plus are for workspaces with a single member**, such as the **Personal** workspace created when you sign up. **A workspace with a Go subscription cannot invite members.**" (`services/www/src/docs/content/console/go.mdx:33-34`); repeated at `services/www/src/docs/content/console/members.mdx:20`: "Workspaces with an [OpenCode Go](/console/go) subscription cannot invite members. Create a separate workspace for your team." Older docs: "Only one member per workspace can subscribe to OpenCode Go." (`packages/web/src/content/docs/go.mdx:57`).
- **Assessment:** a single human subscriber running many automated agents through their own key is not barred by the *seat* clause (the seat restriction is about workspace membership); but multiple humans/swarm operators multiplexing one Go key "on behalf of" each other conflicts with the "own internal use… not on behalf of or for the benefit of any third party" clause and the single-member product rule.

**(b) Client alteration / reverse proxying / header tampering**

- **No ToS clause names reverse proxying or header manipulation specifically — NOT FOUND** (searched the full ToS text for `reverse|proxy|alter|header|User-Agent`; only hits are the ones below).
- Closest ToS clauses:
  - Restriction (m): "decompiles, reverse engineers, or otherwise attempts to obtain the source code or underlying ideas or information of or relating to the Services." (index.tsx:187-189) — note this reaches "the Services" (hosted), not the MIT repo code (carve-out at index.tsx:48-51).
  - Restriction (b): "violates any law or regulation… or any other purpose not reasonably intended by OpenCode" (index.tsx:147-150).
  - Restriction (d): "creates, maintains, or uses accounts in bulk, or creates, maintains, or uses multiple accounts to circumvent usage limits, access restrictions, billing obligations, promotions, suspensions, or any other restriction or policy applicable to the Services" (index.tsx:158-161).
  - Termination: "OpenCode has the sole right to decide whether you are in violation of any of the restrictions set forth in these Terms." (index.tsx:336-337).
- **Client etiquette is contractual-adjacent, in Go docs** (`services/www/src/docs/content/console/go.mdx:74-84`): "OpenCode Go is designed for [OpenCode] and other coding agents that produce similar types of requests. **Traffic is monitored for abuse** that degrades the experience for other users. Your client should: 1. Send typical coding agent traffic 2. **Identify itself with its own user agent, such as `my-coding-agent/1.0`, rather than a generic SDK or HTTP-library name.** 3. Send a stable session ID in `x-opencode-session` for each conversation so we can optimize routing and prompt caching." The same page publishes a **Validated clients** table (Hermes, Claude Code, Codex, ZCode, Pi, jcode, Kilo Code CLI — console/go.mdx:86-101) and a **Known problematic clients** table (console/go.mdx:105-117).
- **Assessment:** modified/third-party clients are *expressly contemplated* and permitted if they follow the etiquette (own User-Agent, stable `x-opencode-session`). What conflicts with documented requirements: spoofing another client's identity, stripping/rotating session identifiers to defeat attribution, or generic-library User-Agents. Any of those, plus proxy-multiplexing, would engage restriction (b) "not reasonably intended by OpenCode" and the abuse monitoring; a fork shipping a Go plugin should keep the etiquette intact (§4).

**(c) Headless / high-frequency automation & abuse/DoS clauses**

- Restriction (j): "runs Maillist, Listserv, any form of auto-responder or 'spam' on the Services, or **any processes that run or are activated while you are not logged into the Services, or that otherwise interfere with the proper working of the Services (including by placing an unreasonable load on the Services' infrastructure)**" (index.tsx:176-181).
- Restriction (e): "automatically or programmatically extracts data or Output (defined below)" (index.tsx:162).
- Restriction (k): "'crawls,' 'scrapes,' or 'spiders' any page, data, or portion of or relating to the Services or Content (through use of manual or automated means)" (index.tsx:182-184).
- Restriction (d) (bulk accounts to circumvent limits) and (g) below also apply.
- **Assessment:** unattended/headless use is not per se prohibited (API-key users are authenticated; Go docs design for agent traffic), but a 50-agent continuous swarm can plausibly "interfere with the proper working of the Services" or "plac[e] an unreasonable load" (j), and bulk automated harvesting of Output engages (e)/(k). Hard usage ceilings are enforced regardless: dollar-value caps with 5-hour/weekly/monthly windows (`console/go.mdx:113-119`; older doc: "5 hour limit — $12 of usage… Weekly limit — $30… Monthly limit — $60", `packages/web/src/content/docs/go.mdx:74-77`) and "Traffic is monitored for abuse" (console/go.mdx:75). Violations are grounds for termination (index.tsx:197-199) and OpenCode "has the sole right to decide whether you are in violation" (index.tsx:336-337).

**(d) Reselling / pooling / sharing of API access**

- The governing clause: "You will only use the Services **for your own internal use, and not on behalf of or for the benefit of any third party**…" (index.tsx:131-132).
- Supporting: restriction (d) (index.tsx:158-161, circumventing billing/access via multiple accounts); restriction (g): "**uses Output to develop artificial intelligence models that compete with the Services or any Third Party Models**" (index.tsx:165-167) — directly relevant to training/scraping-swarm plans; restriction (f): "Represent that the Output was human-generated when it was not" (index.tsx:163).
- Product-level sharing exists but is workspace-internal: "Service accounts and their API keys belong to the workspace…" (`services/www/src/docs/content/console/members.mdx:61`); workspace budgets cover "members and its service accounts" (`services/www/src/docs/content/console/budgets.mdx:3`).
- **Assessment:** exposing Go endpoints through a shared gateway to third parties, team members outside the single-member workspace, or paying customers = use "on behalf of / for the benefit of a third party" → contract breach, independent of MIT (MIT covers the code, never the hosted service contract).

**Other clauses the fork should note:** "We retain all right, title and interest in and to the Services. Except as expressly set forth herein, no rights to the Services or Third Party Models are granted to you." (index.tsx:194-196); "You may not assign, delegate or transfer these Terms…" (index.tsx:395); Delaware governing law (index.tsx:404) and arbitration in New Castle County, Delaware with class-action waiver (index.tsx:423, 464-469); injunctive relief carve-out for "copyrights, trademarks, trade secrets, patents or other intellectual property rights" (index.tsx:443-447).

### 3.4 Code grounding — how the client talks to Zen/Go today (integration points)

All paths relative to `/home/john/Projects/opencode`:

| Integration point | Location | Detail |
|---|---|---|
| Zen provider (SDK level) | `packages/ai/src/providers/opencode-zen.ts:6,21` | `const baseURL = "https://opencode.ai/zen/v1"`; auth `AuthOptions.bearer(input, "OPENCODE_API_KEY")`; `id = ProviderID.make("opencode")` |
| Models.dev catalog entries | `packages/core/src/models-dev/snapshot.txt` (single-line JSON) | provider `opencode-go` → `{name: "OpenCode Go", npm: "@ai-sdk/openai-compatible", api: "https://opencode.ai/zen/go/v1", env: ["OPENCODE_API_KEY"], doc: "https://opencode.ai/docs/go"}`; provider `opencode` → `"OpenCode Zen", api: "https://opencode.ai/zen/v1"` |
| Catalog loader/plugin | `packages/core/src/models-dev.ts:16,300-301,328-347`; `packages/core/src/plugin/models-dev.ts:28` (plugin id `"opencode.models.dev"`) | Bundled snapshot import + live fetch from models.dev with `User-Agent: App.useragent(app)`; cache key `models-dev:catalog:${Hash.fast(source)}` supports alternate catalog sources |
| Console account integration | `packages/core/src/plugin/provider/opencode.ts:21-22` | `defaultServer = "https://opencode.ai/console"`, `clientID = "opencode-cli"`; device-auth (`${server}/auth/device/code`), pulls remote providers/websearch/MCP/policy config from Console (`RemoteResponse`, opencode.ts:25-41); websearch via `${server}/api/websearch` (opencode.ts:369-380) |
| Zen key handling / free tier | `packages/core/src/plugin/provider/opencode.ts:319-341` | `OPENCODE_API_KEY` env or stored connection; without a key the `opencode` provider is enabled with `apiKey: "public"` (free models) and paid models disabled |
| Session/telemetry headers on every LLM request | `packages/core/src/session/model-request.ts:264-275` | sends `x-opencode-session-id`, `x-opencode-parent-session-id`, `x-session-affinity`, `X-Session-Id`, `x-parent-session-id`, `User-Agent`, `x-opencode-project`, `x-opencode-session`, `x-opencode-client` |
| User-Agent format | `packages/core/src/app.ts:24-26` | `opencode/${app.channel}/${app.version}/${app.name}` |
| Generate-path session header | `packages/core/src/generate.ts:70` | default `http.headers: { "x-opencode-session": SessionID.create() }` |
| `/connect` dialog | `packages/app/src/providers/connect/dialog.tsx:232,453-455` | featured providers `["opencode-go", "opencode", "anthropic", "openai", "google", "openrouter", "vercel"]`; "A Go service-account key still belongs to the `opencode-go` integration (zen/go/v1)" |
| CLI auth ordering | `packages/cli/src/commands/handlers/auth/login.ts:21`, `packages/cli/src/commands/handlers/auth/shared.ts:22` | `opencode-go` sorted first in provider login |
| SaaS gateway (server side, also MIT code in this monorepo) | `packages/console/app/src/routes/zen/` (`util/handler.ts:108` reads `x-opencode-session`, `:250` strips it when forwarding upstream), `packages/console/function/src/log-processor.ts:16-20` (Go path allowlist) | The Zen/Go inference proxy itself |
| Legal pages (server side) | `packages/console/app/src/routes/legal/terms-of-service/index.tsx`, `.../privacy-policy/` | Source of the ToS analyzed above |

---

## 4. Clean-room safe harbor plan (decoupling from Go/Zen)

Principle: the fork ships as a **neutral MIT client** — no default code path, catalog entry, or branding points at Anomaly-hosted services; Go/Zen become user-opted, key-bearing, BYOK-style integrations whose users accept Anomaly's ToS directly. All surfaces below are real, cited code.

### 4.1 Native BYOK providers (no fork of the network layer needed — the surface already exists)

- **Config schema:** `packages/schema/src/config/provider.ts` — `ConfigProvider.Info` (line 86-95): `canonical`, `name`, `env`, `package`, `settings`/`headers`/`body` overlays (lines 33-40), `models` map with cost/limits/capabilities/variants (lines 48-82). This is the declarative surface for any provider.
- **Custom/OpenAI-compatible providers:** docs `packages/web/src/content/docs/providers.mdx:375-405` (Atomic Chat custom provider: `npm: "@ai-sdk/openai-compatible"`, `options.baseURL`, arbitrary provider ID), `:1614-1647` (Ollama at `http://localhost:11434/v1`), `:1428-1448` (LM Studio), `:1361-1385` (llama.cpp), `:1570+` (vLLM-style `http://localhost:8000/v1` example — the `OpenAI-compatible` package pattern covers **vLLM, LiteLLM, and any gateway**), `:34-43` ("You can customize the base URL for any provider by setting the `baseURL` option. This is useful when using proxy services or custom endpoints"), `:1757-1790` (OpenRouter via `/connect`).
- **Workspace-level BYOK:** `services/www/src/docs/content/console/providers.mdx:17-29` ("Pick a provider from the catalog and enter its API key", incl. Azure resource+key) — SaaS console feature; the fork can keep it against its own backend or drop it.

### 4.2 Catalog decoupling

- The `opencode`/`opencode-go` provider entries live in the bundled snapshot `packages/core/src/models-dev/snapshot.txt` and are refreshed via `packages/core/src/models-dev.ts:282` ("…and refreshed via"). Fork action: strip both entries (or the whole snapshot) and point `ModelsDev` at a fork-controlled catalog URL — `models-dev.ts:300-301` already parameterizes the source (`models-dev:catalog:${Hash.fast(source)}`). Per CONTRIBUTING.md:20-22 upstream providers are added to `anomalyco/models.dev`; the fork must not depend on that repo's cadence. (`models.dev` dataset licensing is `[UNVERIFIED]` — not examined in this track.)

### 4.3 Console/Go integration as an optional, compliant plugin

- Keep the code shape of `packages/ai/src/providers/opencode-zen.ts` (configurable `baseURL`, `OPENCODE_API_KEY` bearer auth) as an **opt-in** "OpenCode Go" provider module that only activates when the user supplies a key (mirroring the activation logic pattern at `packages/core/src/plugin/provider/opencode.ts:319-341`).
- Preserve Go's client etiquette when talking to `opencode.ai/zen/go/v1`: fork must send **its own** User-Agent (`packages/core/src/app.ts:24-26` builds `opencode/<channel>/<version>/<name>`; set `app.name` to the fork's identity — do not spoof upstream's) and a stable `x-opencode-session` per conversation (`packages/core/src/session/model-request.ts:273`), per `console/go.mdx:78-84`.
- Ship a doc in the plugin stating that Go use is governed by Anomaly's Terms of Use (opencode.ai/legal/terms-of-service), single-member workspace (console/go.mdx:33-34), and that the fork is not a licensee of Go beyond what Anomaly grants users directly.
- Explicitly forbid in the plugin's docs: multi-human key pooling (ToS index.tsx:131-132), reselling via a Heretek gateway (same), and using Go/Zen Output to train competing models (ToS restriction (g), index.tsx:165-167) — training/scraping swarms must use self-hosted/BYOK backends (Ollama/vLLM/LiteLLM).

### 4.4 Hosted-SaaS code and defaults

- The monorepo contains the entire Zen/Go SaaS (`packages/console/**`, `services/www/**`, `infra/`, `sst.config.ts`). The fork MAY delete or quarantine all of it (MIT permission "sublicense" + no copyleft; the only duty is notice retention, LICENSE:9-11). Nothing in the client requires the Console to function except the `opencode` integration (`packages/core/src/plugin/provider/opencode.ts`) and free-model "public" key path — both must be removed or made pluggable so **no code path dials `opencode.ai` by default**.
- Replace `opencode.ai` defaults: `defaultServer = "https://opencode.ai/console"` (opencode.ts:21), Zen `baseURL` (opencode-zen.ts:6), the `opencode-cli` client ID (opencode.ts:22), and the `opencode-go` featured/ordering preferences (`packages/app/src/providers/connect/dialog.tsx:232`, `packages/cli/src/commands/handlers/auth/login.ts:21`, `shared.ts:22`).
- Schema `$schema` references (`https://opencode.ai/config.json`, seen in docs examples like `providers.mdx:38`) may be repointed to the fork's hosted schema.

### 4.5 Branding

Per §1.3: full rebrand, retain MIT notice verbatim, add unaffiliation disclaimer, no reuse of upstream logos/name as a mark. Attribution obligation is satisfied textually (LICENSE:9-11) and is not a license to use the OPENCODE trademark (registration #8267298 / serial 99379413, Anomaly Innovations Inc.).

---

## Sources

Primary — local repo (`/home/john/Projects/opencode`, branch `v2`, HEAD `119cb15d1d`, remote `https://github.com/Heretek-AI/opencode`):
1. `LICENSE` (MIT, "Copyright (c) 2025 opencode") — lines 1-22.
2. `CONTRIBUTING.md` — lines 12 (issue links to anomalyco/opencode), 20-22 (models.dev policy).
3. `README.md` — lines 14, 56, 61, 69 (anomalyco badges/links).
4. `packages/console/app/src/routes/legal/terms-of-service/index.tsx` — full 523 lines; key quotes at 22, 25-29, 33-36, 48-51, 131-132, 142-196, 197-199, 271-286, 336-337, 395, 404, 423, 443-447.
5. `services/www/src/docs/content/console/go.mdx` — lines 1-11 (pricing), 13, 33-34 (single-member), 36-65 (roster), 74-84 (client etiquette), 86-117 (validated/problematic clients), 113-119 (usage windows), 391-420 (endpoints), 443-477 (privacy/ZDR), 481 (no lock-in).
6. `services/www/src/docs/content/console/members.mdx` — lines 20, 61; `.../budgets.mdx` — lines 3, 14-17; `.../billing.mdx` — lines 55-80; `.../providers.mdx` — lines 17-29.
7. `packages/web/src/content/docs/go.mdx` (older variant) — lines 10, 42, 46-63, 57, 74-77, 199-231, 245-265.
8. `packages/web/src/content/docs/zen.mdx` — lines 10, 40, 44-54, 57-80.
9. `packages/web/src/content/docs/providers.mdx` — lines 25-43, 253-281, 375-405, 1279-1310, 1361-1385, 1428-1448, 1570+, 1614-1647, 1757-1790.
10. `packages/ai/src/providers/opencode-zen.ts` — lines 6, 21.
11. `packages/core/src/models-dev/snapshot.txt` (catalog JSON entries `opencode-go`, `opencode`); `packages/core/src/models-dev.ts` — 16, 282, 300-301, 328-347; `packages/core/src/plugin/models-dev.ts` — 28.
12. `packages/core/src/plugin/provider/opencode.ts` — 21-22, 25-41, 319-341, 369-380.
13. `packages/core/src/session/model-request.ts` — 264-275; `packages/core/src/app.ts` — 24-26; `packages/core/src/generate.ts` — 70.
14. `packages/app/src/providers/connect/dialog.tsx` — 232, 453-455; `packages/cli/src/commands/handlers/auth/login.ts` — 21; `.../auth/shared.ts` — 22.
15. `packages/console/function/src/log-processor.ts` — 16-20; `packages/console/app/src/routes/zen/util/handler.ts` — 108, 250.
16. `packages/schema/src/config/provider.ts` — 33-40, 48-95.

Primary — network/API observations (2026-10-07):
17. `GET https://api.github.com/repos/Heretek-AI/opencode` — fork=true, parent anomalyco/opencode (id 975734319), created 2026-10-07T13:52:52Z, default_branch dev.
18. `GET https://api.github.com/repos/anomalyco/opencode` — fork=false, default_branch dev, created 2025-04-30.
19. `GET https://api.github.com/repos/sst/opencode` — `301 Moved Permanently → https://api.github.com/repositories/975734319`.
20. `GET https://api.github.com/repos/Heretek-AI/opencode/branches/{v2,dev}` — v2 head `119cb15d1d…`, dev head `ecc4916b5a…`.
21. HTTP status probes (2026-10-07): `opencode.ai/terms` → 404; `opencode.ai/legal` → 404; `opencode.ai/legal/terms-of-service` → 200 (content matches repo ToS: "Effective date: Aug 15, 2026", "ANOMALY INNOVATIONS, INC.", "own internal use…"); `opencode.ai/zen` → 200; `opencode.ai/console` → 307; `opencode.ai/docs/go`, `opencode.ai/docs/zen` → 200; `opencode.ai/zen/go/v1/models` → 200.
22. USPTO trademark record — Justia: https://trademarks.justia.com/993/79/opencode-99379413.html (serial 99379413, OPENCODE standard character, Anomaly Innovations Inc., filed 2025-09-08, Cl. 9/42, first-use dates); Trademarkia: https://www.trademarkia.com/opencode-99379413 ("registered trademark (Registration #8267298) … registered on 19 May 2026", "Live/Registered", attorney Julia Spoor Gard, Barnes & Thornburg LLP).
23. TTAB PDF https://ttabvue.uspto.gov/ttabvue/ttabvue-91299649-OPP-1.pdf (verified unrelated: OpenAI OpCo, LLC v. SENSORGPT applicant, Ser. 98/451,729).
24. GitHub search (xd://github): anomalyco/opencode issue search "CLA contributor license agreement" → 0 results; PR search "remove configurable default server" → PR #53731 (merged, 2026-10-07) matching local HEAD commit subject.

Secondary (label accordingly):
25. https://www.aipotluck.org/product/opencode — "The repository was renamed from sst/opencode to anomalyco/opencode and the old URL redirects. Verified 2026-08-13…".
26. https://aiwiki.ai/wiki/opencode — Dax Raad post of 2 Jan 2026 on rebranding to "anomaly".
27. https://github.com/opencode-ai/opencode (xd://github repo_view) — unrelated archived Go project, 13,789 stars.
28. https://uspto.report/company/Opencode-Inc (search snippet only; full record blocked by Cloudflare) — separate "Opencode Inc" applicant exists.

Search trails for NOT FOUND items:
- AUP/Cloud API agreement: case-insensitive grep `acceptable use|AUP|abuse@` over `packages/console/app/src` + English `packages/web/src/content/docs` → 0 hits; `packages/console/app/src/routes/legal/` contains only `terms-of-service/` and `privacy-policy/`; web search `opencode.ai acceptable use policy Go Zen cloud API agreement` → no such page.
- WIPO OPENCODE record: WIPO Global Brand Database (branddb.wipo.int) is JS-query-only and not scrapeable; web searches `WIPO global brand database "OPENCODE" trademark` and `"opencode" USPTO trademark -anomaly` surfaced no foreign/Madrid registration.
- CLA/patent policy: greps over CONTRIBUTING.md/AGENTS.md/SECURITY.md/README.md/.github/github (0 hits) + upstream issue search (0 results).
- TRADEMARK file: repo-wide glob `**/*TRADEMARK*` → none.

## Uncertainties

- USPTO registration number 8267298 is from Trademarkia (aggregator); TSDR primary confirmation was not possible without a USPTO API key/JS client. Justia's page is a stale application snapshot (status 630). The *existence of the application by Anomaly Innovations Inc.* is verified by two independent aggregators and is not in doubt; the *registration status* rests on Trademarkia.
- Whether a second Anomaly "OPENCODE" mark (logo variant) exists: Justia owner page lists two entries but the details page returned a JS shell — `[UNVERIFIED]`.
- "Opencode Inc" third-party USPTO filings: existence from a search snippet only; record not retrievable (Cloudflare) — `[UNVERIFIED]`.
- models.dev dataset licensing (the fork would vendor/replace its snapshot) was not researched in this track — `[UNVERIFIED]`.
- The enforceability characterizations in §3.3 "Assessment" paragraphs are risk judgments from clause text, not legal opinions.
