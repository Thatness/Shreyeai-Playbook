# SHReye AI Playbook v1.0 — The 44× Agentic Operating System
**Last updated:** 2026-04-27 | **Version:** 1.0 | **Status:** Agent-Ready

The single source of truth for how SHReye AI designs, orchestrates, deploys, and scales 44× higher-quality AI agents.

## 1. Philosophy & Mental Models
- North Star: **Understand the Universe → Ship Faster → Stay in Full Control**
- Human-in-the-loop is **sacred**; full autonomy is a tool, never the goal.
- Every agent must be: deterministic, auditable, cost-aware, self-healing.
- 44× rule: Every process must deliver 44× velocity, quality, or leverage.

## 2. The SHReye AI Stack 2026 (Updated April 27)
| Layer              | Tool                          | When to Use                              | Strength                     |
|--------------------|-------------------------------|------------------------------------------|------------------------------|
| GitHub-native PRs  | GitHub Copilot Coding Agent   | Final PRs, repo-integrated work          | Best-in-class GitHub context |
| Local / Multi-model| **OpenClaude v0.7.0**         | Exploration, routing, offline, cost-opt  | 200+ models + Hook Chains    |
| Reasoning          | Grok / o1 / Claude Opus 4.7   | High-stakes architecture                 | Deep reasoning               |
| Orchestration      | OpenClaude gRPC + Hook Chains | Multi-agent swarms                       | Self-healing mesh            |

**OpenClaude v0.7.0 (released today)** – The open-source multi-model coding agent.  
One CLI + VS Code extension that routes across **every major model** with persistent project RAG, Knowledge Graph, Hook Chains for self-healing, deterministic serialization, and native xAI/Grok support.  
Use it for **local iteration + cost routing**; let Copilot handle final GitHub PRs.

**Routing Rule (enforced by AGENTS.md):**
1. Exploration / cheap iteration → OpenClaude + cheap model (DeepSeek, Grok-3-mini)
2. Final implementation → escalate to Opus 4.7 / o1 / Grok
3. GitHub PR → always Copilot Coding Agent

## 3. Issue → Agent → PR Workflow (The Magic Cycle)
1. You (or Copilot) create perfect issue
2. Assign to **Copilot**
3. Agent spins Plan mode → implements → opens draft PR
4. You review → comment → agent iterates instantly
5. Merge with confidence

## 4. Prompt Engineering Playbook
(See `templates/prompts/` for full library)

## 5. Agent Orchestration Patterns
- Supervisor → Worker → Reviewer hierarchy
- Hook Chains (OpenClaude v0.7.0) for self-correction
- Error → Human Escalation protocol

## 6. Repo Hygiene & Standards
- All agent PRs must pass CI + our style guide
- No new dependencies without explicit approval
- Every file touched by agent must be explicitly listed in issue

## 7. Templates & Assets
- Full library in `/templates/`

## 8. Metrics & Iteration
- Weekly retro every Monday
- Track: velocity multiplier, cost per task, human review time

**Contribution:** Open a sub-issue → assign to Copilot.
