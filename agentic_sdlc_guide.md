# Agentic SDLC for Single Developers

> A complete operating model for one developer working with Claude Code (Opus 4.6).
> The developer's role shifts from **writing code** to **specifying intent, reviewing output, and making decisions**.

---

## Part 1: The Agentic SDLC

### Mental Model

```
Traditional:  Think → Code → Test → Debug → Review → Ship
Agentic:      Specify → Delegate → Review → Decide → Ship
```

The developer becomes **architect + product owner + reviewer**. Claude becomes the **implementation engine**.

---

### Phase 1: Specify

**You do this. Claude does not.**

| Activity                 | Output                                                        |
| ------------------------ | ------------------------------------------------------------- |
| Define what to build     | Spec document (acceptance criteria, constraints, edge cases)  |
| Define what NOT to build | Explicit scope boundaries                                     |
| Identify risks           | Security concerns, performance requirements, breaking changes |
| Choose architecture      | Tech decisions Claude should follow, not invent               |

**Quality of your spec = quality of Claude's output. This is the highest-leverage skill.**

#### Spec Template (Minimum Viable)

```markdown
## What
[One sentence: what this feature/fix does]

## Why
[Business context — helps Claude make better tradeoff decisions]

## Acceptance Criteria
- [ ] [Concrete, testable criterion]
- [ ] [Concrete, testable criterion]

## Constraints
- Must follow pattern in [specific file]
- Must not break [specific interface/API]
- Performance: [target if relevant]

## Out of Scope
- [What this is NOT]

## Test Scenarios
- Happy path: [describe]
- Edge case: [describe]
- Error case: [describe]
```

**Rule:** If you can't write the acceptance criteria, you're not ready to delegate.

---

### Phase 2: Delegate

**You prompt. Claude implements.**

#### Delegation Levels (choose per task)

| Level          | You Provide                         | Claude Does                          | Use When                                  |
| -------------- | ----------------------------------- | ------------------------------------ | ----------------------------------------- |
| **Full auto**  | Spec only                           | Plan + implement + test + docs       | Well-understood feature, strong CLAUDE.md |
| **Plan first** | Spec + "propose plan before coding" | Plan → wait for approval → implement | Architectural impact, unfamiliar area     |
| **Guided**     | Spec + approach + reference files   | Implement to your design             | Complex integration, strict patterns      |
| **Pair**       | Running conversation, iterative     | Small increments with feedback       | Exploration, prototyping, debugging       |

#### Session Management

Run parallel Claude Code sessions for independent tasks:

```
Session 1: Implementing feature (spec → code → tests)
Session 2: Writing migration script
Session 3: Updating documentation
```

**Rule:** Never wait for Claude. If one session is running, start specifying the next task.

---

### Phase 3: Review

**You do this. Claude assists.**

#### Review Checklist

```
□ Does it meet ALL acceptance criteria?
□ Does it follow existing codebase patterns?
□ Are there edge cases the tests don't cover?
□ Any security implications? (auth, input validation, data exposure)
□ Will this break anything downstream?
□ Is the error handling complete?
□ Would I be comfortable deploying this at 5pm Friday?
```

#### Using Claude to Review Claude

After implementation, open a NEW session:

```
Review the changes in the current branch against this spec:
[paste spec]

Focus on:
1. Spec compliance — does every acceptance criterion have coverage?
2. Edge cases the implementation might have missed
3. Patterns that deviate from the rest of the codebase
4. Anything I'd regret in production
```

**Why a new session?** Same-session review has confirmation bias — Claude defends its own work. Fresh context produces honest critique.

---

### Phase 4: Decide

**You do this. Claude does not.**

Decisions only humans should make:

- Ship or iterate further
- Architecture tradeoffs with long-term consequences
- Whether a shortcut is acceptable given business context
- Risk tolerance for this specific change
- Priority when multiple approaches are viable

**Rule:** Claude proposes. You dispose.

---

### Phase 5: Ship

**You trigger. Claude automates.**

```
Developer: merge + deploy trigger
Claude (CI): lint → test → build → security scan → deploy
Claude (post-deploy): smoke test verification, monitoring check
```

#### Post-Ship (delegate to Claude)

- Generate changelog entry from commits
- Update API docs if endpoints changed
- Create follow-up tickets for tech debt identified during review

---

### The Full Cycle in Practice

```
09:00  Spec feature A (15 min)
09:15  Delegate feature A to Session 1 (full auto)
09:20  Spec bug fix B (10 min)
09:30  Delegate bug fix B to Session 2 (guided)
09:35  Review yesterday's PR from Session 3
09:50  Session 1 complete → Review feature A
10:10  Request fixes from review → Session 1 continues
10:15  Session 2 complete → Review bug fix B → Approve
10:20  Spec feature C...
```

**Throughput target:** 3-5 features/fixes per day vs. 1-2 in traditional mode.

---

## Part 2: What is a Skill?

### Definition

A **skill** is a repeatable, delegatable unit of work that Claude can execute reliably given the right context. It is the intersection of:

```
Skill = Clear Trigger + Sufficient Context + Defined Output + Quality Criteria
```

### Skill Taxonomy for Developers

| Skill Category | Examples                                                    | Delegation Level |
| -------------- | ----------------------------------------------------------- | ---------------- |
| **Routine**    | CRUD endpoints, unit tests, migrations, DTO creation        | Full auto        |
| **Structured** | API design, refactoring, performance optimization           | Plan first       |
| **Analytical** | Debugging, security audit, code review, root cause analysis | Guided           |
| **Creative**   | Architecture design, system design, UX flows                | Pair             |
| **Judgment**   | Ship/no-ship, priority, risk assessment, tradeoffs          | Human only       |

### Skill Maturity Model

Your goal is to move skills DOWN this list over time:

```
Level 0: You can't delegate it          → You do it manually
Level 1: You delegate with hand-holding → Pair mode, lots of back-and-forth
Level 2: You delegate with a spec       → Plan-first or guided mode
Level 3: You delegate with a trigger     → Full auto, CLAUDE.md handles context
Level 4: It runs without you             → CI/CD automation, scheduled tasks
```

**How to level up a skill:**
1. Do it manually once, noting every decision you make
2. Write those decisions into a prompt (→ prompt library)
3. Refine the prompt over 3-5 uses (→ stable prompt)
4. Encode the stable prompt into CLAUDE.md (→ Level 3)
5. Wire it into CI/CD if applicable (→ Level 4)

### Identifying Your High-Value Skills to Automate First

Ask: "What do I do repeatedly that follows a pattern?" Those are your Level 3 candidates.

Common quick wins:
- Test generation for new code
- PR description writing
- Migration scripts from schema changes
- Boilerplate: new service, new module, new endpoint scaffolding
- Changelog generation
- Dependency update + test validation

---

## Part 3: CLAUDE.md — The Developer's Operating System

### What CLAUDE.md Is

CLAUDE.md is a **persistent instruction file** at the root of your repository. Claude Code reads it automatically on every session. It is:

- Your codebase's "brain dump" for Claude
- The single most important file for agentic coding quality
- The difference between Claude guessing and Claude knowing

### What CLAUDE.md Is NOT

- Documentation for humans (though humans can read it)
- A place for general coding principles Claude already knows
- Static — it should evolve with your codebase

---

### CLAUDE.md Structure

```markdown
# CLAUDE.md

## Project Overview
[What this project is, in 2-3 sentences. Business context helps Claude
make better tradeoff decisions.]

## Tech Stack
- Language: TypeScript 5.x (strict mode)
- Framework: NestJS 10
- Database: PostgreSQL 15 via Prisma ORM
- Auth: Keycloak (OIDC)
- Queue: BullMQ on Redis
- Testing: Jest (unit), Supertest (integration)
- CI: GitHub Actions
- Deployment: Kubernetes via Helm

## Project Structure
```
src/
├── modules/           # Feature modules (one directory per domain)
│   ├── users/         # Example: users.module.ts, users.service.ts, users.controller.ts
│   └── ...
├── common/            # Shared utilities, guards, interceptors, decorators
├── config/            # Configuration loading and validation
├── database/          # Prisma schema, migrations, seed scripts
└── main.ts
```

## Code Conventions

### Naming
- Files: kebab-case (user-role.service.ts)
- Classes: PascalCase (UserRoleService)
- Functions/variables: camelCase
- Constants: UPPER_SNAKE_CASE
- Database tables: snake_case plural (user_roles)
- API routes: kebab-case plural (/api/v2/user-roles)

### Patterns to Follow
- Every module follows the pattern in src/modules/users/ — use it as reference
- Services contain business logic. Controllers are thin (validation + delegation only)
- All database access goes through Prisma services, never raw SQL in business logic
- DTOs use class-validator decorators for input validation
- All endpoints require @UseGuards(AuthGuard) — no unauthenticated endpoints
- Errors use NestJS built-in exceptions (NotFoundException, ForbiddenException, etc.)
- Pagination uses cursor-based pattern from src/common/pagination/

### Patterns to AVOID
- Do NOT use barrel exports (index.ts re-exports)
- Do NOT put business logic in controllers
- Do NOT use raw SQL outside of migrations
- Do NOT create circular module dependencies
- Do NOT use default exports (always named exports)

## Testing Requirements
- Unit tests: every public service method, minimum 80% branch coverage
- Integration tests: every API endpoint (happy path + auth + validation errors)
- Test files live next to source: user.service.spec.ts beside user.service.ts
- Use factories from test/factories/ for test data — never hardcode IDs or values
- Mock external services (Keycloak, Redis), never mock internal modules

## Database Conventions
- Migrations via: npx prisma migrate dev --name <descriptive-name>
- Every table has: id (UUID), created_at, updated_at
- Soft delete via deleted_at column where applicable
- Tenant isolation: every query MUST include tenant_id in WHERE clause
- Index naming: idx_<table>_<columns>

## API Conventions
- Versioned: /api/v2/...
- Auth: Bearer token (JWT from Keycloak), validated by AuthGuard
- Response envelope: { data: T, meta?: { cursor, hasMore } }
- Error response: { statusCode, message, error }
- Rate limiting: default 100 req/min per user, annotate exceptions

## Git Conventions
- Branch: feature/<ticket-id>-<short-description>
- Commits: conventional commits (feat:, fix:, chore:, refactor:, docs:, test:)
- PR titles match commit convention
- Squash merge to main

## Common Commands
```bash
npm run dev              # Start dev server
npm run test             # Run all tests
npm run test:watch       # Watch mode
npm run test:cov         # Coverage report
npm run lint             # ESLint
npm run prisma:migrate   # Run pending migrations
npm run prisma:generate  # Regenerate Prisma client
npm run prisma:studio    # Open Prisma Studio
npm run build            # Production build
```

## Known Issues / Tech Debt
- user.service.ts has a N+1 query in getUsersWithRoles() — needs dataloader
- Legacy endpoints in /api/v1/ are deprecated, do not extend them
- Redis connection pooling is not configured for production load
- Some older modules lack integration tests — add when touching them

## Domain-Specific Context
[Anything Claude needs to know about your business domain that isn't
obvious from the code. E.g.:]
- A "Schulträger" is a school authority that manages multiple schools
- Tenant = one Schulträger. All data is tenant-scoped.
- Users can belong to multiple tenants with different roles per tenant
- Academic year boundaries matter for data visibility rules
```

---

### CLAUDE.md Anti-Patterns

| Don't                          | Why                                                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------------- |
| "Write clean code"             | Claude already knows this. Wasted tokens.                                                     |
| "Follow SOLID principles"      | Generic. Tell Claude your *specific* patterns instead.                                        |
| Copy-paste entire style guides | Too long, dilutes signal. Extract only what's non-obvious.                                    |
| Document every file            | CLAUDE.md isn't a codebase map. Focus on patterns and conventions.                            |
| Set and forget                 | Update when conventions change, new patterns emerge, or Claude keeps making the same mistake. |

### CLAUDE.md Maintenance Rule

**Every time Claude gets something wrong because it lacked context, add that context to CLAUDE.md.**

This is how CLAUDE.md evolves from "good enough" to "Claude knows this codebase as well as I do."

---

### Nested CLAUDE.md Files

For monorepos or complex projects, use directory-scoped files:

```
repo/
├── CLAUDE.md              # Global: tech stack, git conventions, shared patterns
├── services/
│   ├── user-service/
│   │   └── CLAUDE.md      # Service-specific: domain rules, local patterns
│   └── notification-service/
│       └── CLAUDE.md
├── packages/
│   └── shared-lib/
│       └── CLAUDE.md      # Library-specific: API surface, versioning rules
└── infrastructure/
    └── CLAUDE.md          # IaC conventions, naming, tagging
```

Claude Code reads the nearest CLAUDE.md in scope. Global context flows down, specific context stays local.

---

## Part 4: Putting It All Together

### Week 1 Checklist for a New Developer

```
□ Install Claude Code, verify API access
□ Write CLAUDE.md for your primary repo (use template above)
□ Pick 3 prompts from the prompt library, customize for your stack
□ Complete one full cycle: Specify → Delegate → Review → Ship
□ Note every time Claude lacked context → update CLAUDE.md
□ Run one "Claude reviews Claude" session on your own PR
```

### Maturity Indicators

| Week | Indicator                                                                           |
| ---- | ----------------------------------------------------------------------------------- |
| 1-2  | Developer can delegate routine tasks without babysitting                            |
| 3-4  | CLAUDE.md covers 80% of "Claude got it wrong" scenarios                             |
| 5-6  | Developer runs 3+ parallel sessions per day                                         |
| 7-8  | Full features delivered spec-to-merge in single day                                 |
| 9-12 | Developer specs more than they review; review is the bottleneck, not implementation |

### The One Rule

**If you're typing code that follows a pattern, you're doing Claude's job. If you're making a decision Claude can't make, you're doing yours.**