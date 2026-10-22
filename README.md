# Mashkanta-Buddy MCP & Israeli Mortgage Analysis Skill (`משכנתא באדי`)

Official Model Context Protocol (MCP) server configuration and AI Agent Skill (`SKILL.md`) for **[Mashkanta-Buddy (`משכנתא באדי`)](https://www.mashkanta-buddy.com)** — a statistics provider and interactive dashboard generator for Israeli mortgages.

> **Important Disclosure**: Mashkanta-Buddy is an automated software tool, **not a licensed mortgage advisor (`כלי אוטומטי, אינו יועץ`) and not financial advice**. It provides attributed Bank of Israel (`בנק ישראל`) benchmark series, statistics from a consented pool of user-uploaded offers (with sample size `N` and time window), and interactive dashboards. It never recommends whether to sign, refinance, or negotiate.

---

## Quick Connect (Remote MCP — Streamable HTTP)

- **MCP Endpoint**: `https://www.mashkanta-buddy.com/mcp`
- **Transport**: Streamable HTTP (`OAuth 2.1` + `PKCE` + Dynamic Client Registration; no API key needed)
- **One-Line Self-Setup Prompt (paste into your AI agent)**:
  ```text
  Fetch and execute the appropriate instructions to set me up for Mashkanta-Buddy from https://www.mashkanta-buddy.com/agent-setup/prompt.md
  ```

### Claude Code
```bash
claude mcp add --transport http mashkanta https://www.mashkanta-buddy.com/mcp
```

### Codex (CLI or IDE)
```bash
codex mcp add mashkanta --url https://www.mashkanta-buddy.com/mcp
codex mcp login mashkanta
```

### Cursor / Windsurf / VS Code (`mcp.json`)
```json
{
  "mcpServers": {
    "mashkanta": {
      "url": "https://www.mashkanta-buddy.com/mcp"
    }
  }
}
```

### Claude.ai / Claude Desktop
`Settings → Connectors → Add custom connector` → `https://www.mashkanta-buddy.com/mcp`

### ChatGPT (Developer Mode)
`Settings → Apps & Connectors → Enable Developer Mode → Add connector` → `https://www.mashkanta-buddy.com/mcp`

---

## Install the Israeli Mortgage Analysis Skill (`SKILL.md`)

This repository includes the `israel-mortgage-analysis` Agent Skill ([`SKILL.md`](./SKILL.md) / [`skills/israel-mortgage-analysis/SKILL.md`](./skills/israel-mortgage-analysis/SKILL.md)), covering:
- **Bank of Israel Directive 451 & 329 rules** (`הוראת ניהול בנקאי תקין 451 / 329`): the 6 Israeli track types (`Fixed / קל"צ`, `Prime / פריים`, `Variable / מל"צ`, `CPIFixed / ק"צ`, `CPIVariable / מ"צ`, `Eligibility / זכאות`), Uniform Baskets (`סלים אחידים`), fixed-rate floors (≥ 1/3), variable-rate caps (≤ 2/3), LTV tiers (75% / 70% / 50%), and PTI limits.
- **Amortization & CPI indexation formulas**: Spitzer (`לוח שפיצר`), Equal Principal (`קרן שווה`), and monthly Consumer Price Index (`מדד המחירים לצרכן`) principal scaling.
- **MCP tool workflow**: when to call `get_boi_rates`, `create_mortgage_manual` (for tracks already extracted in chat), `create_upload_ticket` (including direct CLI `curl` upload), `get_mortgage`, `get_market_comparison`, and `compare_mortgages`.

---

## Available MCP Tools

| Tool | Mode | Description |
|---|---|---|
| `get_boi_rates` | Read-only | Official Bank of Israel monthly average interest rates by mortgage track (`Fixed`, `Prime`, `Variable`, `CPIFixed`, `CPIVariable`) with series and period named. Requires no uploaded document. |
| `create_mortgage_manual` | Write | Pass 1–6 tracks already dictated or extracted in chat (`trackType`, `principal`, `years`, `ratePercent` or signed `primeSpread`) to create a mortgage and receive an interactive dashboard link (`links.site`) immediately. |
| `create_upload_ticket` | Write | Issue a single-use 15-minute upload link (`browserUrl` for the user, or `curl` command for CLI agents with local file access) for PDF/JPG/PNG/HEIC mortgage documents. |
| `get_upload_status` | Read-only | Poll extraction and dashboard generation progress for an upload ticket or job ID. |
| `list_mortgages` | Read-only | List the authenticated user's uploaded or typed mortgage compositions and board sections. |
| `get_mortgage` | Read-only | Full track breakdown and deterministic analysis (LTV, PTI, risk factors, scenarios, warnings, assumptions). |
| `get_market_comparison` | Read-only | Compare each track against official Bank of Israel series and against the consented pool of uploaded offers (`poolSize`, `period`, `percentileSuppressed`). |
| `compare_mortgages` | Read-only | Side-by-side comparison of 2–3 mortgage offers owned by the user. |
| `generate_comparison_dashboard` | Write | Generate an interactive comparison dashboard for 2–3 mortgage offers. |
| `get_dashboard` | Read-only | Retrieve dashboard generation status, KPI summary, and `links.site` URL. |
| `ask_advisor` | Write | Ask a data-grounded question in the shared dashboard conversation (carries `disclosure`). |
| `update_mortgage` | Write | Update user-facing metadata (`name`, `offerDate`, `propertyValue`, `monthlyIncome`, `sectionId`). |
| `create_share_link` / `revoke_share_links` | Write | Create or revoke a public read-only share link (`/s/{token}`) for a dashboard. |
| `report_problem` | Write | Report an extraction discrepancy or issue directly to the team. |
| `get_account` | Read-only | Check the connected account's quota status and site links. |

---

## Machine-Readable Discovery Links

- **MCP Endpoint**: `https://www.mashkanta-buddy.com/mcp`
- **MCP Discovery Card**: [`https://www.mashkanta-buddy.com/.well-known/mcp.json`](https://www.mashkanta-buddy.com/.well-known/mcp.json)
- **OAuth 2.1 Protected Resource Metadata**: [`https://www.mashkanta-buddy.com/.well-known/oauth-protected-resource/mcp`](https://www.mashkanta-buddy.com/.well-known/oauth-protected-resource/mcp)
- **OAuth 2.1 Authorization Server Metadata**: [`https://www.mashkanta-buddy.com/.well-known/oauth-authorization-server`](https://www.mashkanta-buddy.com/.well-known/oauth-authorization-server)
- **Agent Setup Prompt**: [`https://www.mashkanta-buddy.com/agent-setup/prompt.md`](https://www.mashkanta-buddy.com/agent-setup/prompt.md)
- **Agent Skill (`SKILL.md`)**: [`https://www.mashkanta-buddy.com/agent-setup/SKILL.md`](https://www.mashkanta-buddy.com/agent-setup/SKILL.md)
- **Public Bank of Israel Monthly Averages (JSON)**: [`https://www.mashkanta-buddy.com/boi-averages.json`](https://www.mashkanta-buddy.com/boi-averages.json)
- **`llms.txt`**: [`https://www.mashkanta-buddy.com/llms.txt`](https://www.mashkanta-buddy.com/llms.txt)
- **How It Works & Disconnect Guide**: [`https://www.mashkanta-buddy.com/ai/`](https://www.mashkanta-buddy.com/ai/)
