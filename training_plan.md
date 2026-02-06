# Training Plan: Autonomous SDLC with Claude Code

## Phase 1: Foundations (Month 1-2)

### Week 1-2: Tool Proficiency

- Every developer installs Claude Code, gets API access
- Mandatory pairing sessions: each dev works through 3-5 real tickets using Claude Code with a buddy
- Establish baseline: measure time-to-PR, code quality, and rework rates before and after

### Week 2-4: Prompt Engineering for Code

- Teach CLAUDE.md / project-level context files — this is the single highest-leverage skill
- Train on task decomposition: breaking epics into Claude-executable chunks
- Key pattern: spec → implementation → test → review loop, all driven by Claude Code
- Introduce /init, /compact, multi-file editing, and MCP server integrations

### Month 2: Agentic Patterns

- Move from "Claude as autocomplete" to "Claude as autonomous agent"
- Train developers to write detailed specifications (not code) and let Claude Code execute
- Introduce headless/CI mode for automated tasks (test generation, migration scripts, boilerplate)
- Each team picks one real feature to deliver fully agentic as a proof of concept

---

## Phase 2: Process Integration (Month 3-4)

### Redefine the SDLC Roles

- **Developer** becomes spec author + reviewer, not implementer
- **Claude Code** handles: implementation, unit tests, documentation, PR descriptions
- **Human** handles: architecture decisions, acceptance criteria, code review, production validation

### Concrete Process Changes

- Every Jira ticket gets a structured spec template (context, acceptance criteria, constraints, test cases)
- Introduce agentic CI pipelines: Claude Code runs in CI for automated refactoring, test backfill, dependency updates
- Set up MCP servers for your stack (Jira, GitHub, Datadog, etc.) so Claude has full context

### Quality Gates

- Mandatory human review of all agentic PRs — non-negotiable until Month 5
- Track metrics: agentic PR acceptance rate, bugs introduced, review turnaround time

---

## Phase 3: Scaling & Autonomy (Month 5-6)

- Reduce human review to architectural decisions and security-sensitive code
- Developers should be running 3-5 parallel Claude Code sessions per day
- Introduce "M-shaped contributor" evaluation criteria — breadth across frontend/backend/infra/data enabled by agentic tooling
- Automate routine maintenance: dependency updates, accessibility fixes, DORA metric improvements

---

## Critical Success Factors

- **CLAUDE.md files are everything.** Invest heavily in per-repo context files. Poor context = poor output = team loses trust in the tool.
- **Spec quality is the new bottleneck.** Train on writing specs, not on writing code. Developers who write vague specs will get vague code.
- **Don't mandate — demonstrate.** Pair your strongest adopters with skeptics. Let results speak.
- **Measure ruthlessly.** Track: PRs/dev/week, time-to-merge, defect escape rate, lines reviewed vs. lines written. You need data to prove this works.
- **Address fear directly.** Some developers will see this as a threat. Frame it as: "Your job shifts from typing code to making decisions. That's a promotion."

---

## Realistic Expectations

| Timeframe  | Expected Outcome                                                  |
| ---------- | ----------------------------------------------------------------- |
| Month 1-2  | Productivity likely drops as people learn new workflows            |
| Month 3-4  | Parity with pre-agentic output, but with broader scope per dev    |
| Month 5-6  | 2-3x throughput increase for well-adapted teams                   |
