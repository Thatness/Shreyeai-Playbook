# Section 5 – Agent Orchestration Patterns

> **Playbook:** [← Back to PLAYBOOK.md](../PLAYBOOK.md)  
> **Section:** 5 of 8 | **Owner:** AI Lead | **Cadence:** Monthly

---

## 5.1 Single Agent vs. Multi-Agent Teams

Choosing the right architecture is the most important orchestration decision. More agents ≠ better results.

```mermaid
graph TD
    Task[New Task] --> Q1{Is task well-defined\nand atomic?}
    Q1 -->|Yes| SA[Single Agent]
    Q1 -->|No| Q2{Can sub-tasks run\nin parallel?}
    Q2 -->|Yes| PA[Parallel Multi-Agent]
    Q2 -->|No| Q3{Are sub-tasks\nsequential?}
    Q3 -->|Yes| Pipeline[Pipeline Multi-Agent]
    Q3 -->|No| Supervisor[Supervisor + Worker Hierarchy]
```

### When to Use Each Architecture

| Architecture | Use Case | Example |
|---|---|---|
| **Single Agent** | Atomic, well-defined tasks | "Write tests for auth module" |
| **Parallel Agents** | Independent sub-tasks | "Simultaneously: write code + write docs + write tests" |
| **Pipeline** | Sequential dependencies | "Research → Summarize → Format → Publish" |
| **Supervisor + Workers** | Complex tasks with quality gates | "Build feature" with Coder + Reviewer + Tester |
| **Swarm** | Exploration / competitive eval | "3 agents propose solutions, pick the best" |

### Architecture Decision Matrix

| Factor | Single | Parallel | Pipeline | Supervisor |
|---|---|---|---|---|
| Task complexity | Low | Medium | Medium | High |
| Setup overhead | Minimal | Medium | Medium | High |
| Token cost | Lowest | Higher | Medium | Highest |
| Output quality | Baseline | High (parallel wins) | Good | Highest |
| Failure recovery | Simple | Independent | Checkpoint | Supervised |

---

## 5.2 Supervisor → Worker → Reviewer Hierarchy

This is our primary pattern for complex coding tasks.

```mermaid
graph TD
    Human[Human: Issue + AGENTS.md] --> Sup[Supervisor Agent]

    Sup -->|"Task: Write code"| W1[Worker: Coder\nGitHub Copilot]
    Sup -->|"Task: Write tests"| W2[Worker: Tester\nGitHub Copilot]
    Sup -->|"Task: Write docs"| W3[Worker: Documenter\nClaude 3.5 Sonnet]

    W1 -->|Output: code diff| Rev[Reviewer Agent\nClaude 3.5 Sonnet]
    W2 -->|Output: test suite| Rev
    W3 -->|Output: docs PR| Rev

    Rev -->|PASS: all criteria met| Merge[Human review + merge]
    Rev -->|FAIL: specific feedback| W1
    Rev -->|FAIL: specific feedback| W2
    Rev -->|FAIL: specific feedback| W3

    Rev -->|ESCALATE: 3+ failures| Human
```

### Supervisor Agent Responsibilities

1. **Decompose** the issue into atomic sub-tasks
2. **Assign** each sub-task to the appropriate worker
3. **Sequence** tasks (parallel where possible, sequential where required)
4. **Aggregate** worker outputs
5. **Escalate** to human when retry limit is reached

### Worker Agent Responsibilities

1. **Accept** a single, well-scoped task
2. **Execute** using available tools
3. **Validate** own output before returning
4. **Report** completion with evidence (test results, file diffs)
5. **Request clarification** if task is ambiguous (do not guess)

### Reviewer Agent Responsibilities

1. **Validate** output against acceptance criteria
2. **Run** automated checks (tests, lints)
3. **Provide specific feedback** (file + line + what to change)
4. **Track retry count** and escalate at threshold
5. **Never approve** output that fails acceptance criteria

---

## 5.3 Error Handling, Self-Correction & Human Escalation

### Error Handling Flow

```mermaid
flowchart TD
    A[Agent takes action] --> B{Action successful?}
    B -->|Yes| C[Log success\nContinue to next step]
    B -->|No| D[Capture error context\nfull error message + stack]
    D --> E{Error type?}
    E -->|Transient: rate limit / timeout| F[Exponential backoff\nRetry up to 3×]
    E -->|Logic error| G[Self-correction attempt\nwith error context]
    E -->|Ambiguity / missing context| H[Request clarification\nfrom supervisor or human]
    E -->|Destructive action blocked| I[STOP\nEscalate immediately]
    F --> B
    G --> B
    H --> J[Wait for response]
    J --> A
    I --> K[Human intervention required]
```

### Self-Correction Prompt Template

When an agent encounters a logic error, it re-prompts itself with:

```
## Previous Attempt Failed

Task: [original task]
Error: [exact error message]
My approach: [what I tried]

## Self-Correction

Step 1: Why did this fail?
[agent reasons about root cause]

Step 2: What alternative approach should I try?
[agent proposes different solution]

Step 3: What am I confident vs. uncertain about?
[agent flags uncertainty]

## Revised Attempt
[agent executes revised approach]
```

### Escalation Triggers

| Trigger | Action |
|---|---|
| 3 consecutive failures on same step | Escalate to human with full context |
| Confidence score < 0.7 on critical step | Request human verification |
| Detected destructive action (delete, drop table, deploy) | STOP, require explicit human approval |
| Cost exceeds per-task budget | Pause and alert |
| Conflicting instructions in context | Request clarification before proceeding |
| Security-sensitive code change | Flag for mandatory security review |

### Human Escalation Message Template

```
## Agent Escalation Required

**Task:** [issue number + title]
**Step that failed:** [specific step]
**Failure count:** [N of 3]

**What I tried:**
1. [attempt 1]
2. [attempt 2]
3. [attempt 3]

**Error detail:**
[exact error]

**What I need from you:**
[ ] Clarification: [specific question]
[ ] Different approach: [what I'd try if you tell me X]
[ ] Take over: [hand off the task]

**Context for handoff:**
[current state of files, what's done, what's not]
```

---

## 5.4 Parallel Agent Patterns

### Fork-Join (Independent Parallel Tasks)

```mermaid
graph LR
    Start[Supervisor: receive task] --> F1[Fork]
    F1 --> A1[Agent 1: write code]
    F1 --> A2[Agent 2: write tests]
    F1 --> A3[Agent 3: write docs]
    A1 --> J[Join: aggregate outputs]
    A2 --> J
    A3 --> J
    J --> Rev[Reviewer: validate all]
    Rev --> Done[PR ready]
```

**Implementation note:** Agents must be given isolated file scopes to avoid conflicts. The supervisor pre-assigns which files each agent owns.

### Swarm (Competitive Evaluation)

```mermaid
graph LR
    Problem[Complex problem] --> A1[Agent 1: Approach A]
    Problem --> A2[Agent 2: Approach B]
    Problem --> A3[Agent 3: Approach C]
    A1 --> Eval[Evaluator Agent\nor Human]
    A2 --> Eval
    A3 --> Eval
    Eval --> Best[Best solution selected]
```

Use the swarm pattern for:
- Architectural decisions
- Algorithm selection
- Prompt optimization (generate N prompts, eval which performs best)

---

## 5.5 LangGraph Implementation Pattern

Our canonical multi-agent implementation uses LangGraph for state management:

```python
from langgraph.graph import StateGraph, END
from typing import TypedDict, List

class AgentState(TypedDict):
    issue: str
    plan: List[str]
    code_output: str
    test_output: str
    review_result: str
    retry_count: int
    escalated: bool

def supervisor(state: AgentState) -> AgentState:
    """Decompose issue into plan"""
    ...

def coder(state: AgentState) -> AgentState:
    """Implement the plan"""
    ...

def tester(state: AgentState) -> AgentState:
    """Write and run tests"""
    ...

def reviewer(state: AgentState) -> AgentState:
    """Review outputs against criteria"""
    ...

def should_retry(state: AgentState) -> str:
    if state["review_result"] == "PASS":
        return "done"
    elif state["retry_count"] >= 3:
        return "escalate"
    else:
        return "retry"

# Build graph
workflow = StateGraph(AgentState)
workflow.add_node("supervisor", supervisor)
workflow.add_node("coder", coder)
workflow.add_node("tester", tester)
workflow.add_node("reviewer", reviewer)

workflow.set_entry_point("supervisor")
workflow.add_edge("supervisor", "coder")
workflow.add_edge("coder", "tester")
workflow.add_edge("tester", "reviewer")
workflow.add_conditional_edges(
    "reviewer",
    should_retry,
    {
        "done": END,
        "retry": "coder",
        "escalate": END  # trigger human notification
    }
)

app = workflow.compile()
```

---

## 5.6 Observability Requirements

Every multi-agent pipeline must emit:

| Signal | What to Log | Tool |
|---|---|---|
| Task start | Issue ID, model, input tokens | LangSmith |
| Each agent action | Tool call + result + latency | LangSmith |
| Retry events | Retry count + error context | LangSmith + alert |
| Escalations | Full context for human | LangSmith + Slack |
| Task completion | Success/fail + total cost + latency | LangSmith + metrics DB |

---

*Section 5 complete | [Next: Section 6 – Repo Hygiene & Standards →](06-repo-hygiene.md)*
