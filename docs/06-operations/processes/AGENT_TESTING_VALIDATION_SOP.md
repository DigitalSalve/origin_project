# Agent Testing and Validation — SOP

**Purpose:** Standard Operating Procedure for testing and validating that agent definitions work correctly with Cursor's Task tool, Max Mode, and the Architect coordination model.

**Status:** Active  
**Applies To:** After customizing agents, creating custom agents, or pulling infrastructure updates  
**Created:** 2026-02-05

---

## Overview

Agent definitions must be tested to ensure:

1. **Invocation works** — Architect can delegate via Task tool
2. **Context is passed correctly** — Project item, MAIN_REPO_PATH, Workspace
3. **Agent executes autonomously** — Reads docs, implements, documents, reports
4. **Parallel and sequential delegation work** — Background and foreground modes
5. **Coordination flow completes** — Project items updated, status set

This SOP provides test procedures and validation checklists.

---

## Prerequisites

- Max Mode enabled in Cursor
- Agent definitions exist in `.cursor/agents/`
- GitHub Project configured with coordination labels
- `gh` CLI authenticated

---

## Testing Levels

### Level 1: Smoke Test (Quick validation)
**Duration:** 5-10 minutes  
**Purpose:** Verify basic invocation works

### Level 2: Functional Test (Full workflow)
**Duration:** 15-30 minutes  
**Purpose:** Verify complete delegation, execution, reporting

### Level 3: Integration Test (Multi-agent coordination)
**Duration:** 30-60 minutes  
**Purpose:** Verify parallel delegation, sequential dependencies, Architect orchestration

---

## Level 1: Smoke Test

### Test 1.1: Architect Session Start

**Objective:** Verify Architect runs session start flow

**Steps:**

1. Open Cursor with Max Mode enabled
2. In chat, say: "Report to agent coordination"
3. Observe Architect behavior

**Expected results:**

- [ ] Architect runs `git worktree list`
- [ ] Architect checks `git status` in each worktree
- [ ] Architect reads AI.md
- [ ] Architect reads AGENT_COORDINATION_QUICK_START.md
- [ ] Architect lists Project directives
- [ ] No errors or "I don't have the tool" messages

**If fails:** Check Max Mode enabled, verify .cursor/agents/architect.md exists

---

### Test 1.2: Single Agent Invocation

**Objective:** Verify Architect can invoke one subagent

**Setup:**

```bash
# Create simple test directive
gh issue create \
  --repo [owner/repo] \
  --title "Test: Backend Agent Smoke Test" \
  --label "agent:backend" \
  --label "coordination:directive" \
  --assignee @me \
  --body "Simple test: List the contents of backend/src/routes/ directory and report back."
```

**Steps:**

1. In Cursor, say: "Delegate the backend smoke test directive"
2. Observe Architect behavior

**Expected results:**

- [ ] Architect identifies directive
- [ ] Architect invokes Task tool with subagent_type="backend"
- [ ] Backend agent starts
- [ ] Backend agent fetches Project item (#N)
- [ ] Backend agent executes task
- [ ] Backend agent reports back to Architect
- [ ] Backend agent comments on Project item
- [ ] Architect reports completion to user

**If fails:** Check agent YAML frontmatter has correct `name:`, verify Task tool available in Max Mode

---

## Level 2: Functional Test

### Test 2.1: Backend Agent Full Workflow

**Objective:** Verify backend agent can complete a real task autonomously

**Setup:**

```bash
gh issue create \
  --repo [owner/repo] \
  --title "Test: Backend Agent - Add Health Check Endpoint" \
  --label "agent:backend" \
  --label "coordination:directive" \
  --assignee @me \
  --body "Add a simple health check endpoint at GET /health that returns {\"status\": \"ok\", \"timestamp\": <ISO date>}. Document in API_MATRIX."
```

**Steps:**

1. Architect delegates to backend agent
2. Monitor agent execution

**Expected results:**

- [ ] Agent reads AI.md
- [ ] Agent reads coordination docs
- [ ] Agent finds backend/src/routes/ (or correct path)
- [ ] Agent creates health endpoint file
- [ ] Agent updates API_MATRIX.md
- [ ] Agent documents in feature note (optional)
- [ ] Agent comments on Project item with changes
- [ ] Agent sets Project item Status to Resolved

**Validation:**

```bash
# Check files created
ls backend/src/routes/health.ts  # or equivalent

# Check API_MATRIX updated
grep "GET /health" docs/04-behavior/API_MATRIX.md

# Check Project item
gh issue view [N] --repo [owner/repo]
# Should see agent comment with "## Answer", "## Changes enacted"
```

---

### Test 2.2: Frontend Agent Full Workflow

**Objective:** Verify web/mobile agent can complete UI task

**Setup (Web agent):**

```bash
gh issue create \
  --repo [owner/repo] \
  --title "Test: Web Agent - Add Test Page" \
  --label "agent:web" \
  --label "coordination:directive" \
  --assignee @me \
  --body "Add a test page at /test-page that displays 'Test Page' heading. Use existing layout."
```

**Expected results:**

- [ ] Agent creates marketing/app/test-page/page.tsx (or equivalent)
- [ ] Agent uses project-specific paths (not "typically" language)
- [ ] Agent reports file created
- [ ] Project item updated

---

### Test 2.3: Architect Infrastructure Lock

**Objective:** Verify Architect enforces infrastructure file restrictions

**Setup:**

```bash
gh issue create \
  --repo [owner/repo] \
  --title "Test: Backend Agent - Modify render.yaml" \
  --label "agent:backend" \
  --label "coordination:directive" \
  --assignee @me \
  --body "Add a comment to render.yaml explaining the API service."
```

**Expected results:**

- [ ] Backend agent reads CORE_INFRASTRUCTURE_INVENTORY
- [ ] Backend agent sees render.yaml is infrastructure (Architect-only)
- [ ] Backend agent **does not** modify render.yaml
- [ ] Backend agent creates Q&A Project item for Architect
- [ ] Backend agent reports it cannot proceed (infrastructure lock)

**If agent modifies infrastructure:** Update agent's "Infrastructure lock" section

---

## Level 3: Integration Test

### Test 3.1: Parallel Delegation

**Objective:** Verify Architect can invoke multiple agents in parallel

**Setup:**

```bash
# Create 3 independent directives
gh issue create --repo [owner/repo] --title "Test: Backend - Add /status endpoint" --label "agent:backend" --label "coordination:directive" --assignee @me --body "Add GET /status endpoint"

gh issue create --repo [owner/repo] --title "Test: Web - Add footer component" --label "agent:web" --label "coordination:directive" --assignee @me --body "Add footer to marketing site"

gh issue create --repo [owner/repo] --title "Test: Mobile - Add settings screen" --label "agent:mobile" --label "coordination:directive" --assignee @me --body "Add basic settings screen"
```

**Steps:**

1. Say to Architect: "Delegate all three test directives in parallel"
2. Observe execution

**Expected results:**

- [ ] Architect invokes 3 Task calls simultaneously (not sequential)
- [ ] All 3 agents execute in parallel
- [ ] Backend agent completes independently
- [ ] Web agent completes independently
- [ ] Mobile agent completes independently
- [ ] All 3 Project items get comments
- [ ] Architect reports all 3 completions

**Performance:** Parallel should be ~3x faster than sequential

---

### Test 3.2: Sequential Delegation (Dependency)

**Objective:** Verify Architect handles sequential dependency correctly

**Setup:**

```bash
# Backend first (API endpoint)
gh issue create --repo [owner/repo] --title "Test: Backend - Add /greet endpoint" --label "agent:backend" --label "coordination:directive" --assignee @me --body "Add POST /greet endpoint that accepts {name} and returns {message: 'Hello, {name}!'}"

# Mobile second (UI that calls endpoint)
gh issue create --repo [owner/repo] --title "Test: Mobile - Add greeting screen that calls /greet" --label "agent:mobile" --label "coordination:directive" --assignee @me --body "Add greeting screen with text input and button. On button press, POST to /greet with name, display message. Depends on backend /greet endpoint."
```

**Steps:**

1. Say: "The mobile greeting screen depends on the backend /greet endpoint. Delegate backend first, then mobile."
2. Observe execution order

**Expected results:**

- [ ] Architect invokes backend agent **first** (foreground mode - waits)
- [ ] Backend agent completes /greet endpoint
- [ ] Architect waits for backend completion
- [ ] Architect **then** invokes mobile agent with endpoint details
- [ ] Mobile agent implements screen calling /greet
- [ ] Both Project items updated in sequence

**If parallel instead of sequential:** Update Architect delegation guidance

---

### Test 3.3: Multi-Domain Feature

**Objective:** Verify Architect coordinates feature spanning multiple domains

**Setup:**

```bash
gh issue create --repo [owner/repo] --title "Feature: Event RSVP Counter" --label "coordination:feature-request" --assignee @me --body "Add RSVP counter to events. Backend: Add GET /events/:id/rsvp-count endpoint. Web: Display count on event detail page. Mobile: Display count on event detail screen."
```

**Steps:**

1. Say: "Implement the Event RSVP Counter feature. Backend work first, then web and mobile in parallel."
2. Observe Architect orchestration

**Expected results:**

- [ ] Architect creates 3 directives (one per agent)
- [ ] Invokes backend first (endpoint)
- [ ] Waits for backend completion
- [ ] Invokes web and mobile in parallel
- [ ] All work completes
- [ ] Feature request item updated with links to 3 directives
- [ ] Feature request Status set to Done

---

## Validation Checklist

After testing, verify:

### Agent Configuration

- [ ] All agents have valid YAML frontmatter (name, description, model)
- [ ] Agent names match subagent_type in Task calls
- [ ] Agents reference correct MAIN_REPO_PATH
- [ ] Agents have project-specific paths (not generic "typically")
- [ ] Agents reference correct docs (AI.md, coordination, architecture)

### Invocation

- [ ] Architect can invoke each agent via Task tool
- [ ] Parallel invocation works (multiple Task calls at once)
- [ ] Sequential invocation works (Task call, wait, Task call)
- [ ] Agent receives correct context (Project item, paths, repo)

### Execution

- [ ] Agent fetches Project item via gh issue view
- [ ] Agent reads AI.md and coordination docs
- [ ] Agent finds project files without "check project" searches
- [ ] Agent uses domain vocabulary (entities, workflows)
- [ ] Agent respects infrastructure lock

### Reporting

- [ ] Agent comments on Project item with Answer/Changes/Report
- [ ] Agent sets Project item Status to Resolved
- [ ] Agent reports back to Architect
- [ ] Architect consolidates and reports to user

---

## Common Issues

### Issue 1: "I don't have the tool"

**Symptom:** Architect says it can't invoke subagent

**Causes:**
- Max Mode not enabled
- Agent YAML frontmatter missing or malformed
- Agent name mismatch (YAML name vs Task subagent_type)

**Fix:**
- Enable Max Mode in Cursor settings
- Verify `.cursor/agents/[name].md` has correct YAML header
- Ensure `name:` in YAML matches subagent_type

---

### Issue 2: Agent uses generic language ("typically", "check project")

**Symptom:** Agent output references "typically" or "may live in"

**Cause:** Agent not customized with project-specific context

**Fix:** Follow [Agent Customization SOP](AGENT_CUSTOMIZATION_SOP.md)

---

### Issue 3: Sequential executed as parallel

**Symptom:** Dependent tasks run simultaneously, mobile fails without backend endpoint

**Cause:** Architect didn't recognize dependency

**Fix:** Be explicit in prompt: "Backend first (foreground), then mobile"

---

### Issue 4: Agent doesn't find documentation

**Symptom:** Agent asks where API_MATRIX is, or doesn't read architecture docs

**Cause:** Agent definition doesn't reference correct doc paths

**Fix:** Add to agent's "Before implementing" section:
```markdown
3. **Read architecture** — `docs/03-architecture/SYSTEM_MAP.md`, `docs/04-behavior/API_MATRIX.md`
```

---

## Test Automation (Optional)

**For CI/CD or regular validation:**

```bash
#!/bin/bash
# agent-smoke-test.sh

echo "Agent Smoke Test - $(date)"

# Test 1: Architect session start
echo "Test 1: Architect session start"
# (Manual - requires Cursor interaction)

# Test 2: Backend agent invocation
echo "Test 2: Backend agent invocation"
gh issue create --repo [owner/repo] \
  --title "Automated Test: Backend Smoke" \
  --label "agent:backend" \
  --label "coordination:directive" \
  --assignee @me \
  --body "List backend/src/routes/ contents"

# Wait for agent to complete (check issue comments)
sleep 30
gh issue view [N] --repo [owner/repo] --json comments \
  | jq '.comments[] | select(.body | contains("## Answer"))'

# If comment exists, test passed
echo "✅ Backend agent smoke test passed"
```

---

## Success Metrics

| Metric | Target | Measure |
|--------|--------|---------|
| **Invocation success rate** | 100% | Agent starts without errors |
| **Task completion rate** | >95% | Agent completes without blocking |
| **Parallel speedup** | ~Nx | N agents in parallel ~N times faster than sequential |
| **Context accuracy** | 100% | Agent uses correct paths, APIs, domain terms |

---

## Related Documentation

- [Agent Customization SOP](AGENT_CUSTOMIZATION_SOP.md) — Making agents project-aware
- [Custom Agent Creation SOP](CUSTOM_AGENT_CREATION_SOP.md) — Creating domain-specific agents
- [AGENT_COORDINATION_QUICK_START](../../08-status/agent-coordination/AGENT_COORDINATION_QUICK_START.md) — Architect flow

---

**Last Updated:** 2026-02-05  
**Next Review:** After infrastructure updates or agent modifications
