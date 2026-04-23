# Digividual SDD Toolkit
**Advanced Analytica — Internal Framework**

Spec-Driven Development toolkit for building, deploying, and operating Digividuals — persistent AI digital workers exposed as Agent-as-a-Service via MCP servers.

---

## What is SDD?

Spec-Driven Development treats the specification as the source of truth. The agent (Codex, Claude Code or equivalent) receives structured, high-quality context and builds to it. **You act as architect**: design, supervise, review, accept or request changes. **You do not tell the agent how to do its job**.

**Key principle:** Give the agent context it doesn't have. Let it make low-level decisions.

---

## Toolkit Structure

```
digividual-sdd-toolkit/
├── README.md                        ← You are here
├── agents.md                        ← Coding agent instructions (copy to repo root)
│
├── constitution/                    ← Project-level specs (one per Digividual product line)
│   ├── mission.md                   ← Why, what, for whom
│   ├── tech-stack.md                ← Engineering decisions and constraints
│   └── roadmap.md                   ← Living phased delivery plan
│
├── specs/
│   ├── _template/
│   │   ├── digividual-spec.md       ← Digividual specification template (fill per worker)
│   │   └── feature-spec.md          ← Feature spec template (fill per feature)
│   └── example/
│       └── accounts-payable-spec.md ← Worked example: AP Clerk Digividual
│
├── process/
│   ├── sdd-workflow.md              ← The SDD loop: Plan → Build → Validate → Replan
│   └── validation-checklist.md     ← Pre-delivery validation gate
│
└── scripts/
    └── validate-spec.py             ← Checks spec completeness before agent handoff
```

---

## Quick Start

### For a new Digividual

1. **Fill the Constitution** — complete `constitution/` files for the product line (once per project)
2. **Fill the Digividual Spec** — copy `specs/_template/digividual-spec.md`, complete all 6 sections
3. **Run validation** — `python scripts/validate-spec.py specs/your-spec.md`
4. **Hand to agent** — provide constitution + digividual spec as context to Claude Code
5. **Plan the first feature** — fill `specs/_template/feature-spec.md` for Phase 1
6. **Build → Validate → Replan** — follow `process/sdd-workflow.md`

### For an existing Digividual

1. Fill a `feature-spec.md` for the next feature
2. Hand to agent with current constitution + digividual spec
3. Validate output against the spec
4. Update `roadmap.md`

---

## Digividual Architecture (Advanced Analytica Stack)

```
Client (Claude / ChatGPT / VS Code)
        │
        ▼
   MCP Server (FastMCP)          ← Digividual's public interface
        │
        ▼
   FastAPI (Semantic Layer)      ← Business logic, workflow orchestration
        │
   ┌────┴─────────────┐
   ▼                  ▼
SQLite / Milvus Lite  External APIs (ERP, Slack, etc.)
(State + Memory)
        │
        ▼
   Docker / DigitalOcean Droplet
```

Every Digividual is a containerised FastAPI + FastMCP service. Clients invoke via MCP tools. State persists in SQLite. Semantic memory in Milvus Lite.

---

## Naming Convention

| Asset | Convention | Example |
|---|---|---|
| Digividual spec | `{role}-spec.md` | `accounts-payable-spec.md` |
| Feature spec | `{role}-{feature}-spec.md` | `accounts-payable-ingestion-spec.md` |
| MCP server | `{role}-mcp` | `accounts-payable-mcp` |
| Docker image | `aa/{role}-digividual` | `aa/accounts-payable-digividual` |
| DigitalOcean Droplet | `dv-{role}-{env}` | `dv-accounts-payable-prod` |
