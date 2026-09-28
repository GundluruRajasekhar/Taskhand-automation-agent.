# Requirements: Taskhand Agentic Workflow

## 1. Purpose

Taskhand is an AI agent for project and task management. It takes a goal in plain language, chooses its own tools, checks the result of every action, and changes its plan when something goes wrong. It replaces fixed step-by-step automation (such as RPA scripts) with a reasoning loop.

## 2. Definition

An agentic workflow is a multi-step process where an autonomous agent uses reasoning, tools, and decision logic to reach a goal without a hardcoded script. It adapts in real time to intermediate results, context, and feedback.

| Traditional automation | Agentic workflow |
| --- | --- |
| Fixed, linear steps | Plan changes at run time |
| Fails or stops on unexpected input | Observes the failure and re-plans |
| Rules written by a developer | Decisions made from goal and context |

## 3. Functional requirements

### 3.1 Agent loop
- FR-1: Accept a goal as free text.
- FR-2: Break the goal into an ordered plan of tool calls.
- FR-3: For every step, record a thought (why), an action (which tool and inputs), and an observation (what came back).
- FR-4: After each observation, decide whether to continue, re-plan, or finish.
- FR-5: Stop when the goal is met or after a fixed retry limit, then hand control to a person.

### 3.2 Tools
- FR-6: `create_task(title, priority)` adds a task.
- FR-7: `assign_task(id, owner)` assigns a task and rejects the call if the owner is at capacity.
- FR-8: `check_workload()` returns who is over capacity.
- FR-9: `summarize()` returns counts of done, open, and overdue tasks.
- FR-10: `flag_overdue(ids)` flags late tasks and proposes new due dates.

### 3.3 Adaptive behavior
- FR-11: When `assign_task` is rejected, choose the owner with the most spare capacity and retry.
- FR-12: When `check_workload` finds overload, move the lowest-priority tasks first, then verify.
- FR-13: When `summarize` finds overdue tasks, insert a flagging step before the report is finished.

### 3.4 Interface
- FR-14: Show the loop as a step-by-step log, with re-planned steps visually distinct.
- FR-15: Show team capacity and the task list, updated after every step.
- FR-16: Provide Run, Pause, Step once, and Reset controls.
- FR-17: Provide example goals the user can pick in one click.

## 4. Non-functional requirements

- NFR-1: The demo runs from a single `index.html` with no build step.
- NFR-2: Every agent decision is visible and explainable in the log.
- NFR-3: Keyboard accessible, visible focus, respects reduced motion, works on mobile widths.
- NFR-4: Retry limit of three passes to prevent infinite loops.
- NFR-5: No secrets or API keys in client code.

## 5. Agent components

| Component | Role |
| --- | --- |
| Planner | Turns a goal into a queue of tool calls |
| Executor | Runs one tool call and returns an observation |
| Observer | Compares the result with what was expected |
| Re-planner | Inserts corrective steps at the front of the queue |
| Memory | Board state and the step log for the current run |
| Tools | The five functions in section 3.2 |

## 6. Technology

- Demo: HTML, CSS, and JavaScript. Planning is simulated with rules so it works offline.
- Production path: replace the rule-based planner with an LLM call (tool use) behind your own backend API. Keep the executor and tools unchanged.

## 7. Acceptance criteria

1. Running "Plan the website launch" produces at least one re-planned step when an owner is full.
2. Running "Rebalance the team workload" ends with no one over capacity.
3. Running "Write the weekly status report" flags overdue tasks before finishing.
4. Every step in the log has a thought and an observation.
5. Reset returns the board and log to the starting state.

## 8. Out of scope

Authentication, persistent storage, real LLM calls, and multi-user editing. These belong to the full platform.
