# AGENTS.md – SHReye AI Playbook Repository

> This file provides instructions for GitHub Copilot (and any other AI coding agent) working in this repository.  
> **Always read this file first before taking any action.**

---

## Repository Purpose

This repository contains the **SHReye AI Playbook v1.0** – the single source of truth for how the SHReye AI team designs, orchestrates, deploys, and scales AI agents and agentic workflows.

Primary artifacts:
- `PLAYBOOK.md` – master playbook document (entry point)
- `AGENTS.md` – this file
- `playbook/` – detailed per-section content
- `.github/ISSUE_TEMPLATE/` – standardized issue templates

---

## Agent Operating Rules

### MUST DO
- Read `PLAYBOOK.md` and the relevant `playbook/*.md` section before editing anything
- Follow the file structure defined in [Section 6.1 of PLAYBOOK.md](PLAYBOOK.md#61-standard-file-structure)
- Use Mermaid diagrams for any new process flows
- Keep Markdown linting clean: no trailing spaces, consistent heading levels, blank lines around headings and code blocks
- Use relative links (not absolute URLs) when linking between files in this repo
- Bump the version number in `PLAYBOOK.md` and `playbook/CHANGELOG.md` for any substantive change
- Open a PR with a clear description citing the issue number it resolves

### MUST NOT DO
- Do NOT delete or overwrite existing sections without explicit instruction
- Do NOT commit secrets, credentials, or API keys
- Do NOT change the versioning scheme or playbook structure without a `playbook` label issue being opened first
- Do NOT remove acceptance criteria checkboxes from issue templates
- Do NOT merge your own PRs – always wait for human review

---

## File Editing Guidelines

| File | Editing Rule |
|---|---|
| `PLAYBOOK.md` | Edit freely within existing sections; add new sections at the end before "Changelog" |
| `AGENTS.md` | Only update if agent workflow or repo structure changes |
| `playbook/*.md` | Each file owns one section – edit only the relevant file |
| `.github/ISSUE_TEMPLATE/*.md` | Preserve all YAML front-matter; do not remove required fields |
| `README.md` | Keep it concise – it is a pointer to `PLAYBOOK.md` |

---

## How to Run / Validate

This is a documentation-only repository. Validation means:

1. **Markdown lint** – All `.md` files must be valid Markdown with no broken links
2. **Mermaid diagrams** – All `mermaid` code blocks must be syntactically valid
3. **Internal links** – All relative links between files must resolve correctly

To check links manually:
```bash
# Find all internal links in markdown files
grep -rn '\[.*\](\..*\.md' .
```

There are no build steps, package managers, or test runners in this repo.

---

## Workflow for This Repo

```
1. Read the issue carefully
2. Identify which playbook section(s) are affected
3. Read those section files (playbook/*.md)
4. Make surgical, minimal changes
5. Update CHANGELOG.md with the change
6. Open PR with issue reference in the description
7. Do NOT merge – wait for human approval
```

---

## Context & References

- **Playbook entry point:** [PLAYBOOK.md](PLAYBOOK.md)
- **Changelog:** [playbook/CHANGELOG.md](playbook/CHANGELOG.md)
- **Issue templates:** [.github/ISSUE_TEMPLATE/](.github/ISSUE_TEMPLATE/)
- **North Star:** Understand the Universe → Ship Faster → Stay in Control

---

*Last updated: 2026-04-27 | SHReye AI Playbook v1.0*
