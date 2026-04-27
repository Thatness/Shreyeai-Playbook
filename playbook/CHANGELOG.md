# Changelog – SHReye AI Playbook

All notable changes to the SHReye AI Playbook are documented here.

This project follows [Semantic Versioning](https://semver.org/):
- **MAJOR** – Fundamental philosophy or workflow change
- **MINOR** – New section or significant section revision
- **PATCH** – Typos, clarifications, template updates

---

## [1.0.0] – 2026-04-27

### Added
- Initial release of the SHReye AI Playbook v1.0
- `PLAYBOOK.md` – Master playbook document with full table of contents
- `AGENTS.md` – Playbook-specific instructions for GitHub Copilot coding agent
- `playbook/01-philosophy.md` – Philosophy & mental models, 44× quality framework, trust levels
- `playbook/02-stack.md` – SHReye AI Stack 2026, multi-model routing strategy
- `playbook/03-issue-agent-pr-workflow.md` – Issue → Agent → PR workflow with 7-component issue formula
- `playbook/04-prompt-engineering.md` – RCTFCE prompt structure, CoT / ReAct / Plan-and-Execute patterns, system prompt library
- `playbook/05-agent-orchestration.md` – Single vs. multi-agent architectures, Supervisor → Worker → Reviewer hierarchy, error handling and escalation
- `playbook/06-repo-hygiene.md` – Standard file structure, git conventions, testing requirements, security and cost guardrails
- `playbook/07-templates.md` – Issue templates, AGENTS.md starter, PR template, sub-issue breakdown template, prompt library (JSON)
- `playbook/08-metrics.md` – Agent velocity metrics, weekly retro process, OKRs for Q2 2026
- `.github/ISSUE_TEMPLATE/agent-task.md` – GitHub issue template for agent-assigned tasks
- `.github/ISSUE_TEMPLATE/bug-report.md` – GitHub issue template for bug reports
- `.github/PULL_REQUEST_TEMPLATE.md` – Standard PR description template
- `README.md` updated to point to PLAYBOOK.md

### Philosophy
- Established "Understand the Universe → Ship Faster → Stay in Control" as our North Star
- Codified the 44× quality multiplier framework
- Defined five-level trust ladder for agent autonomy (Level 0 → Level 4)

---

## [Unreleased]

### Planned for v1.1
- Public marketing version of the playbook
- LangGraph implementation examples (fully runnable)
- GitHub Pages rendering with custom theme
- Video course outline
- Notion export template
- Integration with external platforms beyond GitHub

---

*Format based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)*
