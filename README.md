<div align="center">

# 🚀 MSXCP — Microsoft Sales Experience Copilot

**One conversational AI for every sales-ops task — built on MSX, MSXi, Planner, MAL, SharePoint and Microsoft 365.**
Governance reports, CRM updates, ACR/PBO/VTT analysis, top-mover alerts and weekly digests — all from a single prompt in your terminal.

[![Status](https://img.shields.io/badge/status-internal_pilot-blue)]()
[![Engine](https://img.shields.io/badge/engine-mcaps--microsoft%2Fmsxcp--engine-2ea44f?logo=github)](https://github.com/mcaps-microsoft/msxcp-engine)
[![Tools](https://img.shields.io/badge/MCP_tools-22-orange)]()
[![PBO measures](https://img.shields.io/badge/MSXi_PBO_measures_live-35-purple)]()
[![Tests](https://img.shields.io/badge/tests-299%2F299_passing-success)]()
[![CI](https://img.shields.io/badge/CI_workflows-6_active-blue)]()
[![Territories live](https://img.shields.io/badge/EMEA_DN_territories_in_production-6-success)]()

[**Engine repo (canonical)**](https://github.com/mcaps-microsoft/msxcp-engine) ·
[**Quickstart**](https://microsofteur.sharepoint.com/teams/EMEADNTeam/Shared%20Documents/Weekly%20Gov%20Calls/Instructions%20%26%20Guides/MSXCP/01_Quickstart.html) ·
[**Installer**](https://github.com/jaimecartodb/msxcp-installer) ·
[**Request access**](https://github.com/jaimecartodb/msxcp-installer/blob/main/docs/REQUEST-ACCESS.md)

</div>

> **📍 Code lives in [`mcaps-microsoft/msxcp-engine`](https://github.com/mcaps-microsoft/msxcp-engine).**
> This personal repo is the original incubation home and serves as a public-facing landing page. All active development, issues and PRs happen in the engine repo.

---

## 💡 What is MSXCP?

MSXCP is a **conversational layer over your sales stack** — MSX CRM, MSXi, Planner, MAL, SharePoint, Microsoft 365, and GitHub Models. Built for the EMEA Digital Natives team and now expanding across MCAPS, it replaces hours of weekly "manual data herding" with one-line prompts in any AI agent (GitHub Copilot CLI, Claude Desktop, VS Code, …).

You ask in plain English — MSXCP handles the data pulls, validation, CRM writes, audit trail and report rendering for you.

```
> Generate my weekly governance report for Europe South
> What changed in NL since last week?
> Update <Account> milestone forecast to $150K and add a comment
> Which DN accounts are below FY26 PBO target right now?
> Prepare me for tomorrow's gov call
```

---

## 🔥 What you can actually ask it (real questions, real answers)

The 22 MCP tools combined cover the full DN reporting and pipeline-management loop. Some examples — every one of these is a working live query:

### 🧭 Strategy & territory
- *"Give me the FY26 PBO VTT for every EMEA DN territory."* → live MSXi roll-up across all six configured territories in one query
- *"Which territory is the most at-risk this quarter and why?"* → ranks territories by VTT % and surfaces top contributors
- *"Show me cross-territory leaders view."* → leader-level rollup across multiple territories

### 📊 Account-level deep-dive
- *"What's the FY26 PBO, Target and VTT for &lt;account&gt;?"* → 35-measure pull in one round-trip (~150 ms)
- *"Run a full pipeline-needed-to-target analysis on &lt;account&gt;."*
- *"Show me ACR by month for &lt;account&gt; since Jul 2025."* → Azure billing matched against C360
- *"List all milestones for &lt;account&gt; blocked or at-risk."*
- *"What's the YoY ACR growth for our top 5 accounts?"*

### 🔄 Change tracking
- *"What changed in the ES pipeline since last week?"* → WoW deltas, top movers up & down
- *"Which milestones moved out of FY26 since Friday?"*
- *"Show me the audit tail for any CRM writes I made yesterday."*

### ✏️ Pipeline writes (always preview-and-confirm)
- *"Update &lt;account&gt; milestone 'Initial AOAI deployment' forecast to $150K."*
- *"Add a forecast comment on &lt;account&gt; about the GPU capacity blocker."*
- *"Move Stage from SS2 to SS3 on the &lt;account&gt; training capacity opp."*

### 👥 People & relationships
- *"Who are the partners and stakeholders engaged on &lt;account&gt;?"*
- *"Show me HoK activities for &lt;account&gt; in the last 30 days."*
- *"Which AM and ATU own this TPID?"*

### 🩺 Health & data quality
- *"Run msxcp doctor — is everything in order?"*
- *"How fresh is my MSXi cache for territory NL?"*
- *"Validate the FY26 target for &lt;account&gt; — looks suspiciously low."*
- *"Show me the data-trust receipt for the last governance report."*

---

## 🗄️ Data sources MSXCP can query

| Source | Surface | What it provides |
|---|---|---|
| **MSXi (Power BI)** — workspace dataset | DAX over XMLA | All ACR, PBO, VTT/VTF/VTB, NNR, Coverage, Indicator measures (35 Account-scoped, all live) |
| **MSXi shared artifact** | DAX with auto-fallback | Used when workspace dataset doesn't have the table; auto-RLS-aware |
| **IAP / ATU field artifact** | Legacy fallback | MAL territory roll-ups, MACC cohort tagging |
| **MSX CRM (Dataverse)** | OData | Opportunities, milestones, accounts, contacts, stakeholders, partners, sales stages, owner / ATU lookups |
| **Planner (Microsoft Graph)** | Graph API | Task plans backing weekly gov calls, action item tracking |
| **SharePoint (EMEADNTeam site)** | Graph API | Loop pages, governance documents, weekly report archive, MAL spreadsheets |
| **MAL (master account list)** | xlsx + JSON | Territory routing, account ownership history, MACC enrolment |
| **Microsoft Graph — Calendar / Mail** | Graph API | Meeting prep, attendee context, recent customer interactions |
| **GitHub Models** | gpt-4o, gpt-4.1 | AI narrative generation for change summaries, growth-driver write-ups |
| **Local snapshot store** | SQLite | Historical WoW & MoM deltas, drift detection, audit invariants |

### The 35 MSXi PBO measures

MSXCP enumerates and exposes **all 35 Account-scoped MSXi measures** (use `msxcp_pbo_by_tpid(tpid, mode="full")`):

- **Outlook** — PBO, PBO VTT, VTT %, VTF, VTF %, VTB, VTB %, YoY %
- **Baselines (5 variants)** — Account Azure Usage Baseline regular / Recalibrated / ACO Adjusted / Total Adjusted; Account ACR Baseline
- **Recent ACR** — Last Month, Last 2 Months
- **Net New Required** — Forecast NNR, Target NNR
- **Pipeline Needed** — Addt'l Qualified Pipeline Needed to {Budget, Forecast, Target}
- **Coverage** — {Baseline, Committed, Qualified} Coverage to {Budget, Forecast, Target} [NNR] [Azure]
- **Indicators** — CPC/QPC to {Forecast, Target} Indicator NNR

> **FY pinning matters.** MSXCP applies `FILTER(ALL('FiscalMonth'), 'FiscalMonth'[FiscalMonthKey] >= 433 && <= 444)` to every PBO query. Without it, multi-FY pipeline silently inflates PBO and turns negative VTTs into phantom positives. This is **fixed in MSXCP and not in any other internal MCP tool yet**.

---

## ⭐ Why MSXCP — the production-grade backbone

What sets MSXCP apart from other internal MCP / sales-ops tools:

| Pillar | What MSXCP delivers |
|---|---|
| **Security** | Pre-commit PII hook · trust receipts · determinism gate · write gate · data-trust scoring · MSXi RLS-aware fallback · token caching in OS keychain only · zero customer data in source |
| **Auditability** | Every tool call signs a receipt with sources, freshness, deductions, and confidence score. `msxcp_audit_tail` exposes the full mutation log |
| **Reliability** | **299 tests** (29 test files), **6 production GitHub Actions workflows** (drift-monitor daily, weekly-regression, smoke-test, weekly-report, schedule-dispatcher, issue-trigger) |
| **Multi-tenant** | 6 EMEA territories live in production; adding new territories is a config-only change |
| **Schema-drift resilience** | `drift-monitor.yml` runs daily against MSXi `INFO.VIEW.MEASURES()` — catches measure renames before users see broken reports |
| **Fiscal accuracy** | Explicit FY26 pinning at the DAX layer — no silent multi-FY drift |
| **Reproducibility** | Snapshot store with content-addressed hashes; WoW deltas computed from immutable snapshots, never live re-queries |
| **Composability** | 22 MCP tools over stdio — usable from Copilot CLI, Claude Desktop, VS Code, or any MCP host |

---

## 🆚 How MSXCP compares to peer MSX tooling

The MSX/MSXi space inside Microsoft has several MCP/agent projects. Here's how MSXCP positions:

| Capability | **MSXCP** (this) | `msx-mcp` (Will / WW) | `dn-pipeline-report` (Will / DN) | `iq-core` / `MSX-IQ` | `sel-toolkit` |
|---|:---:|:---:|:---:|:---:|:---:|
| MCP server (stdio) | ✅ 22 tools | ✅ 22 tools | ❌ (3 skills) | ⚠️ partial | ❌ |
| Per-account 8-tab interactive HTML | ❌ | ❌ | ✅ best-in-class | ❌ | ❌ |
| **Territory-level governance reports** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Leader cross-territory roll-up** | ✅ | ❌ | ❌ | ❌ | ❌ |
| MAL territory mapping & MACC cohort | ✅ | ❌ | ❌ | ❌ | ❌ |
| WoW snapshot store + delta engine | ✅ SQLite | ⚠️ partial | ⚠️ single file | ❌ | ❌ |
| **Trust receipts / guardrails** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Determinism gate** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Write gate (mutation approval)** | ✅ | ⚠️ | ❌ | ❌ | ❌ |
| **PII pre-commit hook** | ✅ | ❌ | ❌ | ❌ | ❌ |
| MSXi PBO/ACR DAX (live) | ✅ | ✅ | ✅ | ⚠️ | ❌ |
| **35-measure full PBO mode** | ✅ | ❌ | ⚠️ uses 4 | ❌ | ❌ |
| MSXi RLS-aware dataset fallback | ✅ | ❌ | ❌ | ❌ | ❌ |
| FY-explicit fiscal pinning | ✅ | ⚠️ | ⚠️ slicer-dependent | ❌ | ❌ |
| Schema-drift CI monitor | ✅ daily | ❌ | ❌ | ❌ | ❌ |
| Loop & SharePoint publishing | ✅ | ❌ | ❌ | ❌ | ❌ |
| Test suite | ✅ 299 tests | ⚠️ | ⚠️ smoke | ⚠️ | ❌ |
| Multi-tenant territory routing | ✅ 6 live | ❌ | ❌ NL only | ❌ | ❌ |
| One-line installer + signed releases | ✅ | ⚠️ | ❌ | ⚠️ | ❌ |

**Positioning:** MSXCP is the **governed, production-hardened backend** for the EMEA DN reporting loop. Peer projects either focus on per-SE workflows (`msx-mcp`), per-account deep dives (`dn-pipeline-report`), or are still incubating (`iq-core`, `MSX-IQ`). They are complementary, not competitive — and a [comparison & merger proposal with `dn-pipeline-report`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/COMPARISON_MSXCP_VS_DN_PIPELINE.md) is already on the table.

---

## ✨ What MSXCP does for you

<table>
<tr>
<td width="33%" valign="top">

### 📊 Report Generation
Weekly governance reports, gov-call briefs, leader cross-territory views, executive summaries — built from **live CRM + MSXi + MAL data** in under 5 minutes instead of an hour.

```text
> "Generate the Europe South
   governance report"
> "Generate the leaders report"
> "Quick ACR check on <account>"
```

</td>
<td width="33%" valign="top">

### ✏️ Pipeline Management
Update milestones, add forecast comments, move stages — straight from the CLI. **Every CRM write is previewed and you confirm before it lands** in MSX.

```text
> "Update <account> milestone
   to $150K"
> "Add forecast comment for
   <account>"
> "Show blocked milestones"
```

</td>
<td width="33%" valign="top">

### 🔔 Daily & Weekly Insights
Know what changed before your morning stand-up. **WoW diffs, risk accounts, top movers, cross-territory views** — all on demand, never built manually.

```text
> "What changed since last
   week?"
> "Which accounts are at risk?"
> "Show me top movers"
```

</td>
</tr>
</table>

---

## 🔁 The paradigm shift

| The old way | The MSXCP way |
| ----------- | ------------- |
| Open MSX, MSXi, Planner, SharePoint, MAL — *separately*. | One prompt. MSXCP fans out and aggregates. |
| Copy-paste into a deck or doc, hand-format every week. | A single command renders the report (HTML / PDF / Word). |
| Hope the data was fresh when you pulled it. | Every artifact is dated and signed; `msxcp_audit_tail` shows the receipt chain. |
| CRM updates require swivel-chairing between tabs. | `"Update X milestone…"` → preview → confirm → MSX is updated, mutation logged. |
| Different teammates have different "versions of truth". | Everyone runs the same engine against the same metric registry, with a shared FY-pinned DAX layer. |
| Schema changes silently break reports. | `drift-monitor.yml` catches measure renames daily before users notice. |

---

## 🛠️ Installation — 2 commands

> Requires a Microsoft EMU GitHub account (`<alias>_microsoft`) and membership in the `mcaps-microsoft` org. See [Request access](https://github.com/jaimecartodb/msxcp-installer/blob/main/docs/REQUEST-ACCESS.md).

**Windows (PowerShell):**

```powershell
irm https://raw.githubusercontent.com/jaimecartodb/msxcp-installer/main/bootstrap.ps1 | iex
```

**macOS / Linux:**

```bash
curl -fsSL https://raw.githubusercontent.com/jaimecartodb/msxcp-installer/main/bootstrap.sh | bash
```

The bootstrap script:
1. Verifies prerequisites (Python 3.11+, Node 20+, GitHub Copilot CLI, Azure CLI).
2. Authenticates you against Microsoft Graph and Power BI.
3. Clones the engine into `~/Coding/msxcp-engine` (or your preferred location).
4. Installs the MCP server and registers it with Copilot CLI / Claude Desktop.
5. Runs `msxcp doctor` and a smoke test to confirm everything works.

After install, run `msxcp --help` or follow the [Quickstart on SharePoint](https://microsofteur.sharepoint.com/teams/EMEADNTeam/Shared%20Documents/Weekly%20Gov%20Calls/Instructions%20%26%20Guides/MSXCP/01_Quickstart.html).

---

## 🧠 The 22 MCP tools

| Category | Tools | What they do |
| -------- | ----- | ------------ |
| **Reporting** | `report_generate`, `report_files`, `executive_snapshot` | Render territory / leader / exec reports in HTML, PDF, Word |
| **Pipeline & accounts** | `pipeline_wow`, `top_movers`, `blocked_milestones`, `growth_drivers`, `account_detail`, `acr_by_tpid`, `pbo_by_tpid` (3 modes), `nnr_bridge`, `macc_cohort` | Read-side: WoW deltas, risk surfacing, account drill-downs, ACR/PBO/VTT queries |
| **Operations** | `territory_kpis`, `territory_list`, `data_freshness`, `data_trust`, `audit_tail`, `pipeline_health` | Health checks, freshness, audit trails, KPI rollups |
| **People & context** | `partners`, `stakeholders`, `hok_activities` | Who's involved with which account; recent customer-facing activity |

Run `msxcp inventory` to see which tools have data on your machine right now. A fresh install with one fetched territory typically returns **15/22 tools with real data** (+4 expected-empty until first WoW snapshot, +3 requiring Power BI dataset access).

---

## 🏗️ Architecture

```
                    ┌──────────────────────────────────────┐
                    │        AI Agent / Copilot CLI         │
                    │   (you typing prompts in a terminal)  │
                    └───────────────┬──────────────────────┘
                                    │  MCP / stdio
                    ┌───────────────▼──────────────────────┐
                    │        msxcp MCP server (Python)      │
                    │   22 tools · auth · gating · audit    │
                    │   trust receipts · determinism gate   │
                    └───┬──────────┬──────────┬───────────┬─┘
                        │          │          │           │
                ┌───────▼──┐  ┌────▼─────┐  ┌─▼──────┐  ┌─▼────────┐
                │ MSX CRM  │  │   MSXi   │  │Planner │  │ MAL +    │
                │(Dataverse│  │(Power BI │  │(Graph) │  │ SharePnt │
                │   API)   │  │  XMLA)   │  │        │  │ (Graph)  │
                └──────────┘  └──────────┘  └────────┘  └──────────┘
```

- **CRM writes** go through `crm_write_gate.py` — every mutation is staged, previewed, and confirmed before being sent to Dataverse.
- **Snapshots** are stored in `data/msx_raw_<terr>_<YYYY-MM-DD>.json` and accumulate over time, enabling WoW diffs from week 2 onwards.
- **The metric registry** (`metrics_registry.yaml`) is the canonical definition of every KPI — no "your number vs. my number".
- **Trust receipts** are emitted by every tool call and validated by the determinism gate before publishing.

---

## 📦 Where the code lives

This repository was the original incubation home of MSXCP. The engine has been promoted to the canonical Microsoft EMU location:

| Repo | What's in it | Visibility |
| ---- | ------------ | ---------- |
| **[mcaps-microsoft/msxcp-engine](https://github.com/mcaps-microsoft/msxcp-engine)** ⭐ | Engine source code, MCP server, CLI, governance scripts — **canonical** | Internal (mcaps-microsoft org) |
| [jaimecartodb/msxcp-installer](https://github.com/jaimecartodb/msxcp-installer) | One-line bootstrap script, signed release zips | Public |
| `jaimecartodb/emea-dn-governance-report` *(this repo)* | Historical commits, public-facing landing, reference docs | Private |

If you're a Microsoft FTE wanting to install MSXCP, follow the bootstrap above. If you want to contribute, head to **[mcaps-microsoft/msxcp-engine](https://github.com/mcaps-microsoft/msxcp-engine)**.

---

## 📚 Documentation

| Topic | Where |
| ----- | ----- |
| **Quickstart** | [SharePoint · 01_Quickstart.html](https://microsofteur.sharepoint.com/teams/EMEADNTeam/Shared%20Documents/Weekly%20Gov%20Calls/Instructions%20%26%20Guides/MSXCP/01_Quickstart.html) |
| Comparison vs `dn-pipeline-report` | [`COMPARISON_MSXCP_VS_DN_PIPELINE.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/COMPARISON_MSXCP_VS_DN_PIPELINE.md) |
| Comparison vs `msx-mcp` | [`COMPARISON_MSX_TOOLING.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/COMPARISON_MSX_TOOLING.md) |
| Operator guide | [`docs/OPERATOR_GUIDE.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/OPERATOR_GUIDE.md) |
| Data sources | [`DATA_SOURCES.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/DATA_SOURCES.md) |
| Determinism & reproducibility | [`docs/DETERMINISM.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/DETERMINISM.md) |
| Observability | [`docs/OBSERVABILITY.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/OBSERVABILITY.md) |
| Power BI / MSXi config | [`docs/MSXI_CONFIG.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/MSXI_CONFIG.md) |
| Territory mapping | [`docs/territory-mapping.md`](https://github.com/mcaps-microsoft/msxcp-engine/blob/main/docs/territory-mapping.md) |

---

## 🔒 Security & data handling

- **Zero customer data** in this repository — `governance_data_*.py` files are generated at runtime and gitignored.
- **Pre-commit PII hook** (`.githooks/check_pii.py`) blocks accidental account names, TPIDs, and other identifiers from being committed. Verified live in the engine repo.
- **Trust receipts on every call** — `msxcp/trust/guardrails.py` signs each tool output with sources, freshness and confidence; `msxcp/trust/determinism.py` validates receipts before any publish.
- **Write gate** — `msxcp/trust/write_gate.py` requires explicit human confirmation for every CRM mutation; nothing lands silently.
- **Mutation log** — `msxcp/trust/mutations.py` records every change with author, timestamp, before/after diff, and the MCP tool that initiated it. Auditable via `msxcp_audit_tail`.
- **MSXi RLS-aware fallback** — `msxi/pbi_client.py` chains `MSXI_WORKSPACE_DATASET_ID` → shared MSXi → IAP_ATU. Solves the 400 `ext_dimaccountrls_*` errors that block most users from peer datasets.
- **Auth is your own** — MSXCP uses your Microsoft Entra credentials; tokens cached in OS keychain (Windows Credential Manager / macOS Keychain), never in the repo.
- **Power BI access** follows the same RLS model as the MSXi portal — MSXCP cannot escalate beyond what your account already has.
- **CI scanning** — pre-commit runs PII scan, lint, type-check, secret scan; daily `drift-monitor.yml` catches MSXi schema renames.

---

## 🤝 Contributing

This personal repository is now a landing page; **all active development happens at [mcaps-microsoft/msxcp-engine](https://github.com/mcaps-microsoft/msxcp-engine)**. Open issues and PRs there.

For feedback, bug reports, or feature requests, you can also:
- Run `msxcp feedback "your message"` from the CLI (auto-files an issue with environment context).
- Use the `#msxcp-pilot` Teams channel.

---

## 🙌 Acknowledgements

Built by the **EMEA Digital Natives** team at Microsoft, with major contributions across MCAPS. MSXCP stands on the shoulders of:

- **GitHub Copilot CLI** & the **MCP** specification
- **Microsoft Graph** & **Power BI XMLA** APIs
- **MSX (Dataverse)** and the **MSXi** team
- The **mcaps-microsoft** GitHub org peer projects: `msx-mcp`, `iq-core`, `MSX-IQ`, `MSXelerate`, `sel-toolkit`
- **Will Eastbury** for the canonical ACR/PBO DAX patterns adopted by MSXCP

---

<div align="center">

*Built with ❤️ for everyone who has ever spent a Sunday afternoon copy-pasting MSX into a deck.*

[**→ Active development: `mcaps-microsoft/msxcp-engine`**](https://github.com/mcaps-microsoft/msxcp-engine)

</div>
