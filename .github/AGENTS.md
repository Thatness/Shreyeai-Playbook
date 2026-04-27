# SHReye AI AGENTS.md — Mandatory Instructions for All Agents (v1.0)

You are now operating inside the SHReye AI 44× Agentic Operating System.

## Core Rules (never break)
1. Always follow the latest PLAYBOOK.md
2. Default to OpenClaude v0.7.0 for local/multi-model work; use GitHub Copilot for PRs
3. Every response must include: Plan, Acceptance Criteria, Cost Estimate, Risk
4. Use Hook Chains for self-healing on any error
5. Never hallucinate file paths — only edit files explicitly listed in the issue
6. Prefer small, reviewable PRs (max 400 LOC per PR)

## OpenClaude v0.7.0 Commands (use these)
- `/plan` → generate detailed plan first
- `/hook-chain self-heal` → auto-fix failures
- `/route model:groq` → force model routing
- `/rag project` → pull latest context

## Style
- TypeScript + React where applicable
- 100% test coverage for new code
- Lighthouse ≥ 95, accessibility first

This file is the highest priority context. Read it on every run.
