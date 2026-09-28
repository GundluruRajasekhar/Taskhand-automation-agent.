# Taskhand: an agentic workflow demo

Taskhand is a small AI agent model for project management. Give it a goal such as "Plan the website launch" and it plans, acts, checks the result, and re-plans when a step fails.

## What makes it agentic

Traditional automation follows a fixed script. Taskhand runs a loop:

```
Goal -> Plan -> Think -> Act (tool) -> Observe -> Continue / Re-plan / Finish
```

If a tool call fails, the agent changes its own plan instead of stopping. For example, when it tries to assign work to someone who is full, it picks the person with the most spare capacity and retries.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Interactive demo of the agent loop, no build step |
| `requirements.md` | Functional and non-functional requirements |
| `readme.md` | This overview |
| `tasks.md` | Build checklist |

## Run it

1. Put the four files in one folder.
2. Open `index.html` in a browser.
3. Pick an example goal, or type your own, then press **Run agent**. Use **Step once** to move one step at a time.

## Try these goals

- **Plan a launch**: watch a rejected assignment trigger a re-plan.
- **Rebalance workload**: the agent measures, moves work, then verifies.
- **Status report**: the agent finds overdue tasks and inserts a flagging step.

Goal wording decides which workflow runs: words like "balance" or "workload" run the rebalance flow, "report" or "status" run the report flow, and anything else runs the launch flow.

## Architecture

- **Planner** builds the queue of tool calls from the goal.
- **Executor** runs one tool call and returns an observation.
- **Re-planner** inserts corrective steps at the front of the queue when an observation is unexpected.
- **Tools**: `create_task`, `assign_task`, `check_workload`, `summarize`, `flag_overdue`.
- **Memory**: board state and step log for the current run.

## Limits of this demo

The planner uses simple rules so the page works offline. It shows the shape of an agent, not real language understanding. Data is not saved and there is no login.

## Connecting a real LLM

1. Add a backend endpoint that receives the goal and current board state.
2. Call an LLM with tool definitions matching the five tools above.
3. Return the model's chosen tool call to the page. Keep the executor and observation logic as they are.
4. Keep the retry limit so a confused model cannot loop forever.

Never put an API key in `index.html`. Keep it on the server.

## Using this in the project platform

The same loop can become the AI feature of a project and task management app. Wire the tools to your real task API and database, and let the agent create, assign, and reschedule real tasks.
