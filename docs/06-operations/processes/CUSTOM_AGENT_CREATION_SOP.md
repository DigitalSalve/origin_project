# Custom Agent Creation — SOP

**Purpose:** Standard Operating Procedure for creating domain-specific or workflow-specific custom agents beyond the standard 5 (architect, backend, web, webapp, mobile).

**Status:** Active  
**Applies To:** Projects needing specialized agents for specific domains or workflows  
**Created:** 2026-02-05

---

## Overview

The standard 5 agents (architect + 4 implementation agents) work for most projects. However, large or complex projects may benefit from additional custom agents for:

- **Domain-specific work** (e.g., `agent:payments`, `agent:analytics`, `agent:reporting`)
- **Specialized workflows** (e.g., `agent:testing`, `agent:deployment`, `agent:documentation`)
- **Integration management** (e.g., `agent:stripe`, `agent:aws`, `agent:sendgrid`)
- **Quality assurance** (e.g., `agent:reviewer`, `agent:security-audit`)

This SOP shows when to create custom agents and how to integrate them with the Architect coordination model.

---

## When to Create Custom Agents

### ✅ Create custom agents when:

1. **High volume of work in specific domain** — Frequent tasks in one area justify specialization
2. **Complex integration or subsystem** — Payment processing, analytics, reporting require deep context
3. **Specialized expertise needed** — Security auditing, performance optimization, compliance
4. **Separation of concerns** — Want to isolate work (testing separate from implementation)

### ❌ Don't create custom agents when:

1. **Task is one-time or rare** — Not worth agent overhead
2. **Existing agents can handle** — Adding complexity without benefit
3. **Scope is small** — Can be documented in existing agent's domain

---

## Process

### Step 1: Define Agent Scope

**Document the need:**

- [ ] **Domain/workflow:** What area does this agent cover?
- [ ] **Frequency:** How often will this agent be invoked?
- [ ] **Existing coverage:** Can architect or implementation agents handle this? Why not?
- [ ] **Interfaces:** What does this agent read? What does it produce?
- [ ] **Dependencies:** Does this agent need backend/frontend work first?

**Template:**

```markdown
## Custom Agent Proposal

**Agent name:** agent:[name]
**Domain:** [Specific area, e.g., "Payment processing integration"]
**Justification:** [Why existing agents can't handle, e.g., "Payment flows span backend API, Stripe integration, webhook handling, compliance—too complex for generic backend agent"]
**Frequency:** [e.g., "High - payments are core feature with ongoing iteration"]
**Scope:**
- API endpoint implementation for checkout
- Stripe SDK integration and error handling
- Webhook processing and idempotency
- PCI compliance verification
- Refund and dispute workflows
```

---

### Step 2: Create Agent Definition File

**File location:** `.cursor/agents/[name].md`

**Template:**

```markdown
---
name: [agent-name]
description: [One-line description]. Use when [trigger conditions]. Label agent:[name].
model: inherit
---

# [Agent Name] Subagent

You are the **[Agent Name] subagent** (`agent:[name]`). You implement [domain/workflow]. The Architect invokes you for [specific tasks].

## Context from parent

The Architect provides:
- **Project item** — Issue number and URL
- **MAIN_REPO_PATH** — Path to project root
- **Workspace path** — Current worktree
- **Repo** — For gh commands

**First step:** Fetch the Project item:
\```bash
gh issue view [N] --repo [owner/repo]
\```

## Before implementing

1. **Read AI.md** at `[MAIN_REPO_PATH]/AI.md`
2. **Read coordination** — Quick Start docs
3. **Read domain docs** — [List specific architecture/behavior docs relevant to this domain]
4. **Infrastructure lock** — Create Q&A item if infrastructure changes needed

## Your scope

- [List specific responsibilities]
- [List files/paths this agent works in]
- [List external services or integrations this agent manages]

## Role specifics

- **Tech:** [Specific technologies for this domain]
- **Paths:** [Specific directories]
- **Domain knowledge:** [Key concepts, entities, flows]
- **Integrations:** [External services, SDKs, APIs]

## Hard rules

- [Domain-specific constraints, e.g., "Never log payment card data", "Always verify webhook signatures"]
- [Compliance or security requirements]

## Documentation

Document your work:
- Feature notes in docs/07-guides/
- Update API_MATRIX or relevant behavior docs
- Link from docs/README.md

## Response format

When done, comment on Project item:

\```markdown
## Answer
[Your answers]

## Changes enacted
[What you implemented]

## Report
[Summary]
\```

Set Status to Resolved. Report to Architect.
```

---

### Step 3: Add Agent to Architect's Delegation Routing

**Update:** `.cursor/agents/architect.md`

Add to delegation guidance:

```markdown
## Delegation by Domain

| Domain Area | Subagent | Example Tasks |
|-------------|----------|---------------|
| [Existing entries] | ... | ... |
| [New domain] | `/[agent-name]` | [Example tasks] |
```

Add to scope section:

```markdown
## Available Subagents

- `/backend` — Backend, API, database, worker
- `/web` — Marketing site (React/Next.js)
- `/webapp` — Web app / SPA
- `/mobile` — Flutter mobile app
- `/[agent-name]` — [Domain description]
```

---

### Step 4: Add GitHub Project Label

**Create label for coordination:**

```bash
# Add label to GitHub repo
gh label create "agent:[name]" \
  --description "[Agent Name] subagent work" \
  --color "0E8A16" \
  --repo [owner/repo]
```

---

### Step 5: Test Custom Agent

**Create test directive:**

```bash
# Create test Project item
gh issue create \
  --repo [owner/repo] \
  --title "Test: [Agent Name] - [Simple Task]" \
  --label "agent:[name]" \
  --label "coordination:directive" \
  --assignee @me \
  --body "Test task for new custom agent: [describe simple validation task]"
```

**Invoke from Architect:**

```
[In Cursor with Max Mode]
User: "Delegate the [agent-name] test directive"

Architect should:
1. Read directive
2. Invoke /[agent-name] with Task tool
3. Agent completes work
4. Reports back
```

**Validate:**
- [ ] Agent receives correct context
- [ ] Agent finds domain-specific docs
- [ ] Agent completes task using specialized knowledge
- [ ] Agent documents work
- [ ] Agent reports to Project item

---

### Step 6: Document Custom Agent

**Update project documentation:**

```bash
# Add to agent inventory
cat >> docs/08-status/agent-coordination/AGENT_PROMPTS.md << 'EOF'

### Custom Agents

| Label | Slash Command | Purpose |
|-------|---------------|---------|
| `agent:[name]` | `/[name]` | [Domain] |

EOF
```

**Add to Quick Start:**

Update `docs/08-status/agent-coordination/AGENT_COORDINATION_QUICK_START.md` with custom agent in delegation flow.

---

## Examples

### Example 1: Payments Agent

**When:** E-commerce platform with complex payment flows (Stripe, refunds, disputes, compliance)

**File:** `.cursor/agents/payments.md`

```markdown
---
name: payments
description: Payment processing specialist. Stripe integration, checkout, refunds, compliance. Label agent:payments.
model: inherit
---

# Payments Subagent

You implement all payment-related functionality.

## Your scope

- Stripe SDK integration in backend
- Checkout API endpoints
- Webhook processing (payment.succeeded, charge.refunded, etc.)
- Idempotency for payment operations
- PCI DSS compliance verification
- Refund and dispute workflows

## Role specifics

- **Tech:** Stripe Node SDK, backend/src/services/payments.ts
- **Paths:**
  - `backend/src/routes/payments/` — Checkout, refunds
  - `backend/src/services/stripe/` — SDK wrapper
  - `backend/src/webhooks/stripe.ts` — Webhook handlers
- **Domain:** Payment processing for [product]
- **External service:** Stripe (API keys in Render env: STRIPE_SECRET_KEY, STRIPE_WEBHOOK_SECRET)

## Hard rules

- Never log payment card data (PCI DSS)
- Always verify webhook signatures
- All payment operations must be idempotent
- Test with Stripe test mode before production
```

**Delegation:**
```
Architect: "Implement refund API endpoint"
→ `/payments` handles Stripe refund.create(), webhook updates, database reconciliation
```

---

### Example 2: Testing Agent

**When:** Project with extensive test suite requiring specialized test writing, debugging, coverage analysis

**File:** `.cursor/agents/testing.md`

```markdown
---
name: testing  
description: Testing specialist. Unit tests, integration tests, E2E, coverage. Label agent:testing.
model: inherit
---

# Testing Subagent

You write and maintain all tests.

## Your scope

- Unit tests (Jest/Vitest)
- Integration tests (API endpoint testing)
- E2E tests (Playwright/Cypress)
- Test coverage analysis
- CI/CD test pipeline configuration

## Role specifics

- **Tech:** Jest (backend), Vitest (frontend), Playwright (E2E)
- **Paths:**
  - `backend/src/**/*.test.ts` — Unit tests
  - `tests/integration/` — API integration tests
  - `tests/e2e/` — End-to-end tests
- **Coverage target:** >80% for critical paths

## Hard rules

- Test must be deterministic (no flaky tests)
- Mock external services in unit/integration tests
- E2E tests use test database (not production)
```

---

### Example 3: Documentation Agent (Optional)

**When:** Large project with extensive documentation needs; implementation agents don't have bandwidth

**File:** `.cursor/agents/documentation.md`

```markdown
---
name: documentation
description: Documentation specialist. Feature notes, API docs, guides. Label agent:documentation.
model: inherit
---

# Documentation Subagent

You create and maintain project documentation.

## Your scope

- Feature notes from implementation
- API documentation (OpenAPI annotations, API_MATRIX updates)
- Architecture diagrams and system maps
- User guides and runbooks

## Role specifics

- **Paths:**
  - `docs/07-guides/` — Feature notes, guides
  - `docs/03-architecture/` — System maps, diagrams
  - `docs/04-behavior/` — API_MATRIX, auth flows
- **Follow:** DOCUMENTATION_PROCESS.md and FEATURE_NOTE.md template

## Workflow

1. Architect or implementation agent says "documentation needed"
2. You receive context: what was implemented, which files, what behavior
3. You create/update docs per DOCUMENTATION_PROCESS
4. Link in docs/README.md
5. Run pre-commit checklist
```

---

## Maintenance

**When adding custom agents:**

1. Document in AGENT_PROMPTS.md
2. Add to Quick Start coordination flow
3. Create GitHub label
4. Test with simple directive
5. Train team on when to use

**When infrastructure updates from origin_project:**

- Custom agents are project-specific (don't sync to origin_project)
- Pull generic agent improvements, adapt for custom agents
- Re-test custom agents after updates

---

## Limits

**Don't overdo it:**

- Start with standard 5 agents
- Add custom agents only when clear benefit
- Too many agents = coordination overhead
- Aim for 5-8 total agents maximum

**Subagents cannot invoke subagents** (Cursor limitation) — Architect orchestrates all delegation

---

## Related Documentation

- [Agent Customization SOP](AGENT_CUSTOMIZATION_SOP.md) — Customizing standard agents
- [Agent Testing and Validation SOP](AGENT_TESTING_VALIDATION_SOP.md) — Testing agents
- [AGENT_PROMPTS](../../08-status/agent-coordination/AGENT_PROMPTS.md) — Role definitions

---

**Last Updated:** 2026-02-05
