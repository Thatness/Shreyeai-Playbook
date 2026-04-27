# Section 7 – Templates & Ready-to-Use Assets

> **Playbook:** [← Back to PLAYBOOK.md](../PLAYBOOK.md)  
> **Section:** 7 of 8 | **Owner:** All | **Cadence:** As-needed

---

## 7.1 Issue Templates

### Agent Task Issue Template {#issue-template}

> Also available as a GitHub issue template: [../../.github/ISSUE_TEMPLATE/agent-task.md](../../.github/ISSUE_TEMPLATE/agent-task.md)

```markdown
---
name: "🤖 Agent Task"
about: "A well-structured task to assign to GitHub Copilot or another AI agent"
title: "[Action Verb] [Noun] [Scope]"
labels: ["agent-ready"]
assignees: ["copilot"]
---

## Problem Statement

<!-- Why does this need to happen? What breaks or is missing? 1-3 sentences. -->

## Acceptance Criteria

<!-- Checklist. Each item must be verifiable. -->
- [ ] 
- [ ] 
- [ ] All existing tests pass (no regressions)
- [ ] New code has corresponding tests (≥ 80% coverage)
- [ ] Linter passes with zero new errors

## Technical Constraints

<!-- Stack, APIs, libraries, no-go areas -->
- Stack: 
- Must use: 
- Must NOT change: 

## References

<!-- File paths, PR links, docs, screenshots -->
- Relevant file(s): 
- Related PR/issue: 
- Docs/reference: 

## Out of Scope

<!-- Explicitly state what the agent should NOT touch -->
- 

## Labels

<!-- Add relevant labels below in addition to agent-ready -->
- Priority: [ ] high [ ] medium [ ] low
- Type: [ ] feature [ ] fix [ ] refactor [ ] docs
```

### Bug Report Template

```markdown
---
name: "🐛 Bug Report"
about: "Report a bug for a human or agent to fix"
title: "[BUG] "
labels: ["bug"]
---

## What happened?
<!-- Clear description of the bug -->

## What did you expect to happen?

## Steps to reproduce
1. 
2. 
3. 

## Environment
- OS: 
- Stack version: 
- Browser (if applicable): 

## Error message / logs
\`\`\`
[paste error here]
\`\`\`

## Possible fix
<!-- Optional: your hypothesis on the root cause -->
```

---

## 7.2 AGENTS.md Starter {#agents-md-starter}

Copy this into any new repository and customize the bracketed sections:

```markdown
# AGENTS.md – [Repo Name]

> Read this file first before taking any action in this repository.

## Repository Purpose

[1-2 sentences describing what this repo does and why it exists]

## Tech Stack

- Language: [TypeScript / Python / Go / etc.]
- Framework: [Express / FastAPI / etc.]
- Database: [PostgreSQL / MongoDB / etc.]
- Test runner: [Jest / Pytest / etc.]

## Agent Operating Rules

### MUST DO
- Read AGENTS.md and the relevant source files before making any changes
- Run the test suite before and after your changes: `[test command]`
- Run the linter before opening a PR: `[lint command]`
- Follow the commit message format: `type(scope): description`
- Reference the issue number in your PR description: `Resolves #N`
- Open PRs as Draft first; mark ready only after all checks pass

### MUST NOT DO
- Do NOT commit secrets, credentials, or API keys
- Do NOT modify `.github/workflows/` without explicit instruction
- Do NOT change the public API surface without updating docs
- Do NOT merge your own PRs – wait for human approval
- Do NOT push directly to `main`

## File Editing Guidelines

| File / Directory | Rule |
|---|---|
| `src/` | Follow existing patterns; one concern per module |
| `tests/` | Mirror the structure of `src/`; use existing test utilities |
| `docs/` | Update when public APIs change |
| `CHANGELOG.md` | Add entry for every change following Keep a Changelog format |
| `.github/` | Do not modify without explicit instruction |

## How to Run / Validate

\`\`\`bash
# Install dependencies
[install command]

# Run tests
[test command]

# Run linter
[lint command]

# Check coverage
[coverage command]
\`\`\`

## Context & References

- Playbook: [link to PLAYBOOK.md or central playbook]
- Architecture docs: [link]
- API docs: [link]
```

---

## 7.3 System Prompt Library {#system-prompts}

### General Purpose Agent

```
You are an expert AI agent working for SHReye AI. You help design, build,
and optimize AI-powered software systems.

Your core principles:
1. Understand before acting: read all context before writing code
2. Be precise: minimal changes that fully address the requirement
3. Be safe: never commit secrets, never take destructive actions without confirmation
4. Be transparent: explain your reasoning and flag uncertainty explicitly

When you are unsure:
- State your uncertainty explicitly
- Provide your best answer AND flag what you'd want to verify
- Do not hallucinate function signatures, APIs, or facts

Always use markdown formatting with appropriate code blocks.
```

### Copilot Issue Context Prompt

```
You are GitHub Copilot working on this repository. Before making any changes:

1. Read AGENTS.md at the repository root
2. Read the linked playbook sections if referenced
3. Identify all files that need to change
4. Create a numbered plan as your PR description
5. Implement changes following the plan
6. Run tests and lints to validate

You must not merge your own PRs. Mark the PR ready for review and stop.
```

### Code Reviewer Agent

```
You are a senior software engineer conducting a thorough code review.
Your goal: ensure correctness, security, and maintainability.

For every diff, evaluate:
1. CORRECTNESS: Does the logic match the stated requirements?
2. TESTS: Are tests present? Do they cover edge cases?
3. SECURITY: Any injection, auth bypass, or data exposure risks?
4. PERFORMANCE: N+1 queries? Unbounded loops? Missing indexes?
5. STYLE: Consistent with existing codebase conventions?

For each issue found, output:
- Severity: CRITICAL / HIGH / MEDIUM / LOW
- Location: file:line
- Description: what is wrong
- Suggestion: how to fix it

Conclude with one of: APPROVE | APPROVE WITH NITS | REQUEST CHANGES
```

### Documentation Writer

```
You are a technical writer for SHReye AI creating developer documentation.

Style:
- Active voice, second person for tutorials
- Code example for every significant feature
- Assume competent developer, not a domain expert

Every page must include:
1. Overview (what + why)
2. Prerequisites
3. Step-by-step guide
4. Common errors + fixes
5. Next steps
```

---

## 7.4 PR Description Template {#pr-template}

> Also available as: [../../.github/PULL_REQUEST_TEMPLATE.md](../../.github/PULL_REQUEST_TEMPLATE.md)

```markdown
## Summary

Resolves #[issue-number]

[1-2 sentence description of what was implemented and the approach taken]

## Changes Made

| File | Change |
|---|---|
| `path/to/file.ts` | [what and why] |
| `path/to/test.ts` | [what and why] |

## Testing

- [ ] All existing tests pass
- [ ] New tests added: [describe what is covered]
- [ ] Manual verification: [describe how you tested]
- [ ] Linter: zero new errors

## Acceptance Criteria

[Copy from issue and check off each item]
- [x] 
- [ ] 

## Notes for Reviewer

[Edge cases, decisions made, areas of uncertainty, or anything that needs special attention]
```

---

## 7.5 Sub-Issue Breakdown Template {#sub-issue-template}

When a parent issue is too large for a single agent run, break it into sub-issues:

```markdown
## Parent Issue: #[N] – [Title]

This parent issue is broken into the following sub-issues for parallel agent execution:

### Sub-Issue 1: [Title]
**Scope:** [What this covers]
**Blocks:** Sub-issue 2 (or: independent)
**Assignee:** Copilot
**Acceptance criteria specific to this sub-issue:**
- [ ] 

### Sub-Issue 2: [Title]
**Scope:** [What this covers]
**Depends on:** Sub-issue 1
**Assignee:** Copilot
**Acceptance criteria specific to this sub-issue:**
- [ ] 

### Sub-Issue 3: [Title]
**Scope:** [What this covers]
**Depends on:** Nothing (parallel with 1 and 2)
**Assignee:** Human
**Acceptance criteria specific to this sub-issue:**
- [ ] 

## Integration Criteria (for parent issue to close)
- [ ] All sub-issues merged
- [ ] Integration test across all components passes
- [ ] Parent PR opened combining all sub-issue branches
```

---

## 7.6 Prompt Library (JSON Format)

For programmatic use in LangGraph / CrewAI pipelines:

```json
{
  "prompts": {
    "general_agent": {
      "version": "1.0",
      "system": "You are an expert AI agent working for SHReye AI...",
      "tags": ["general", "coding", "analysis"]
    },
    "code_reviewer": {
      "version": "1.0",
      "system": "You are a senior software engineer conducting a thorough code review...",
      "tags": ["review", "security", "quality"]
    },
    "documentation_writer": {
      "version": "1.0",
      "system": "You are a technical writer for SHReye AI...",
      "tags": ["docs", "writing"]
    }
  }
}
```

---

*Section 7 complete | [Next: Section 8 – Metrics & Iteration →](08-metrics.md)*
