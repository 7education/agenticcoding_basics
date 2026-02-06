# Claude Code Prompt Library — Senior Developers

> 10 battle-tested prompts for agentic software development.
> Copy-paste ready. Designed for Claude Code with Opus 4.6.

---

## How to Use This Library

- **Direct use:** Paste into Claude Code terminal as-is, replacing `[placeholders]`
- **CLAUDE.md integration:** Add relevant prompts to your repo's `CLAUDE.md` as reusable instructions
- **Chaining:** Combine prompts sequentially for full feature delivery (e.g., #1 → #3 → #5 → #7)

---

## 1. Feature Implementation from Spec

**When:** You have acceptance criteria and need production-ready code.

```
Implement the following feature:

## Context
- Repository: [repo-name]
- Service/module: [target module]
- Language/framework: [e.g., TypeScript/NestJS]

## Requirements
[Paste acceptance criteria or user story]

## Constraints
- Follow existing code patterns in [reference file/directory]
- Maintain backward compatibility with [existing API/interface]
- Must not break existing tests

## Deliverables
1. Implementation code
2. Unit tests (minimum 80% branch coverage for new code)
3. Update relevant API documentation if endpoints changed

Start by analyzing the existing codebase structure, then propose your implementation plan before writing code.
```

---

## 2. Code Review & Refactor

**When:** Existing code needs quality improvement or review before merge.

```
Review and refactor the following code with a focus on: [production readiness | performance | maintainability | security].

## Scope
- Files: [file paths or "all changed files in current branch"]
- Priority: [what matters most — e.g., "eliminate N+1 queries" or "reduce cognitive complexity"]

## Review Checklist
- Identify bugs, race conditions, or edge cases
- Flag any security vulnerabilities (injection, auth bypass, data exposure)
- Suggest concrete refactors with code — don't just describe problems
- Check error handling completeness
- Verify naming consistency with codebase conventions

## Output Format
For each finding:
1. File + line reference
2. Severity: CRITICAL | HIGH | MEDIUM | LOW
3. Current code snippet
4. Proposed fix (actual code, not description)

Apply all CRITICAL and HIGH fixes directly. Present MEDIUM and LOW as recommendations.
```

---

## 3. Test Generation

**When:** You need comprehensive test coverage for existing or new code.

```
Generate tests for [file/module path].

## Testing Requirements
- Framework: [Jest | pytest | Go testing | etc.]
- Types needed: [unit | integration | e2e — pick applicable]
- Coverage target: [e.g., "all public methods, 90% branch coverage"]

## Test Strategy
- Happy path for every public method/endpoint
- Edge cases: null/undefined inputs, empty collections, boundary values
- Error paths: invalid input, network failures, timeout scenarios
- Concurrency issues if applicable

## Constraints
- Use existing test fixtures/factories from [path to test helpers]
- Follow AAA pattern (Arrange-Act-Assert)
- No test interdependencies — each test must run in isolation
- Mock external dependencies, not internal modules
- Use descriptive test names: "should [expected behavior] when [condition]"

Do NOT generate trivial getter/setter tests. Focus on behavior and business logic.
```

---

## 4. Database Migration

**When:** Schema changes, data migrations, or performance optimization.

```
Create a database migration for the following change:

## Change Description
[e.g., "Add tenant_id column to users table for multitenancy support"]

## Database
- Engine: [PostgreSQL 15 | MySQL 8 | etc.]
- ORM/migration tool: [Prisma | TypeORM | Alembic | Flyway | raw SQL]
- Estimated affected rows: [e.g., "~4.8M rows in users table"]

## Requirements
- Migration must be reversible (include rollback)
- Must be zero-downtime compatible (no table locks on large tables)
- Include data backfill strategy if populating new columns
- Add appropriate indexes for query patterns: [describe common queries]

## Safety Checks
- Verify migration on current schema before generating
- Flag any destructive operations (column drops, type changes)
- Estimate execution time for large tables
- Include pre/post migration validation queries
```

---

## 5. API Endpoint Design & Implementation

**When:** Building new endpoints or extending existing APIs.

```
Design and implement an API endpoint:

## Endpoint Specification
- Method + Path: [e.g., "POST /api/v2/users/{userId}/roles"]
- Purpose: [what it does in one sentence]
- Auth: [required scopes/roles, e.g., "requires admin or tenant-admin role"]

## Request/Response Contract
- Request body/params: [describe expected input]
- Success response: [expected shape + status code]
- Error responses: [list expected error cases + status codes]

## Implementation Requirements
- Input validation with descriptive error messages
- Idempotency handling if POST/PUT
- Rate limiting considerations: [e.g., "standard tier" or "heavy operation, 10 req/min"]
- Pagination if returning collections (cursor-based preferred)
- Include OpenAPI/Swagger annotation

## Non-Functional
- Target latency: [e.g., "<200ms p95"]
- Log: request metadata, auth context, outcome. Never log PII.
- Emit metrics: request count, latency histogram, error rate

Generate the endpoint, request/response DTOs, validation, tests, and documentation.
```

---

## 6. Debugging & Root Cause Analysis

**When:** Something is broken and you need systematic diagnosis.

```
Diagnose and fix the following issue:

## Symptoms
[Describe what's happening — error messages, unexpected behavior, performance degradation]

## Reproduction
- Steps: [how to trigger the issue]
- Frequency: [always | intermittent | under load]
- Environment: [local | staging | production]

## What Has Been Tried
[List previous debugging attempts so Claude doesn't repeat them]

## Relevant Context
- Recent changes: [last deploy, config change, dependency update]
- Affected files/services: [if known]
- Error logs/stack traces: [paste relevant logs]

## Instructions
1. Analyze the symptoms and form 2-3 hypotheses ranked by likelihood
2. For each hypothesis, describe what evidence would confirm/refute it
3. Investigate the most likely hypothesis first by examining relevant code
4. Once root cause is identified, implement the fix
5. Add a regression test that would have caught this issue
6. Suggest monitoring/alerting to detect recurrence
```

---

## 7. Documentation Generation

**When:** Code needs technical docs, ADRs, or runbooks.

```
Generate documentation for [module/service/system]:

## Documentation Type
[Pick one: API reference | Architecture Decision Record | Runbook | README | Onboarding guide]

## Audience
[e.g., "Senior developers joining the team" or "On-call engineers"]

## Scope
- Components: [list what to cover]
- Depth: [overview | detailed | exhaustive]

## Requirements
- Derive all information from the actual codebase — do not invent or assume
- Include: purpose, architecture, key decisions, dependencies, data flow
- For ADRs: context, decision, consequences, alternatives considered
- For runbooks: step-by-step with exact commands, escalation paths
- For API docs: all endpoints, auth, request/response examples, error codes

## Format
- Markdown
- Include diagrams as Mermaid blocks where they add clarity
- Code examples must be copy-paste executable

Read the relevant source files first, then generate documentation.
```

---

## 8. Security Audit

**When:** Pre-release security check or hardening existing code.

```
Perform a security audit on [scope: file paths, service, or "authentication flow"]:

## Focus Areas
- [ ] Authentication & authorization (broken access control, privilege escalation)
- [ ] Input validation & injection (SQL, NoSQL, XSS, command injection)
- [ ] Data exposure (PII in logs, verbose errors, insecure defaults)
- [ ] Dependency vulnerabilities (check package.json / requirements.txt)
- [ ] Secrets management (hardcoded keys, tokens, connection strings)
- [ ] CSRF/CORS misconfiguration
- [ ] Rate limiting & DoS vectors

## Context
- Auth mechanism: [JWT | session | OAuth2 | Keycloak]
- Data classification: [what sensitive data flows through this code]
- Compliance requirements: [GDPR | SOC2 | BITV | none specific]

## Output Format
For each finding:
1. OWASP category
2. Severity: CRITICAL | HIGH | MEDIUM | LOW
3. Location (file + line)
4. Attack scenario (how it could be exploited)
5. Fix (actual code, not just recommendation)

Apply CRITICAL fixes immediately. Present others as a prioritized remediation list.
```

---

## 9. CI/CD Pipeline & DevOps

**When:** Setting up or improving build, test, and deployment pipelines.

```
[Create | Optimize | Debug] the CI/CD pipeline for this project:

## Current State
- CI/CD platform: [GitHub Actions | GitLab CI | Azure DevOps]
- Build tool: [npm | gradle | docker | etc.]
- Deployment target: [Kubernetes | ECS | Azure App Service | etc.]
- Current pipeline duration: [e.g., "~18 minutes, target <10 minutes"]

## Requirements
- Stages: [lint → test → build → security scan → deploy]
- Environments: [dev | staging | production]
- Deployment strategy: [rolling | blue-green | canary]

## Specific Task
[e.g., "Add parallel test execution to reduce pipeline time" or
"Implement automatic rollback on failed health checks" or
"Add DORA metrics collection (deployment frequency, lead time, MTTR, change failure rate)"]

## Constraints
- Must work with existing [monorepo | multi-repo] structure
- Secrets via [Vault | GitHub Secrets | Azure Key Vault]
- Container registry: [ECR | ACR | GHCR]

Generate the pipeline configuration files, explain non-obvious decisions, and include a test/validation strategy for the pipeline itself.
```

---

## 10. Legacy Code Modernization

**When:** Migrating, refactoring, or replacing legacy systems incrementally.

```
Modernize the following legacy code while maintaining functional parity:

## Scope
- Source: [file/module path]
- Current stack: [e.g., "Express.js with raw SQL queries, no types"]
- Target stack: [e.g., "NestJS with Prisma, full TypeScript"]

## Migration Strategy
- Approach: [strangler fig | big bang | parallel run]
- Phase: [e.g., "Phase 1: Add TypeScript types and interfaces without changing behavior"]

## Requirements
- Zero behavior change in this phase (refactor only, no feature changes)
- Preserve all existing tests — they are the safety net
- Add type definitions for all public interfaces
- Extract hardcoded values to configuration
- Replace callback patterns with async/await where applicable
- Add structured logging to replace console.log

## Validation
- All existing tests must pass without modification
- No new runtime dependencies unless explicitly approved
- Generate a before/after comparison of the public API surface
- Flag any implicit behavior that is not covered by existing tests

Start by mapping the current module's dependencies and public API, then propose the modernization plan before executing.
```

---

## Usage Tips

1. **Always provide context.** Claude Code with repo access is 10x more effective than without. Invest in your `CLAUDE.md`.
2. **Chain prompts for full features:** Spec (#1) → Implement → Test (#3) → Security (#8) → Docs (#7)
3. **Be specific about constraints.** "Follow existing patterns" is vague. "Follow the pattern in `src/modules/auth/auth.service.ts`" is actionable.
4. **Let Claude read first.** Prompts that say "analyze the codebase, then propose a plan, then implement" outperform "just implement this."
5. **Review everything.** These prompts optimize for speed, not blind trust. Human review remains non-negotiable.