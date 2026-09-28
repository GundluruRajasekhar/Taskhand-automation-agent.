# Tasks: Taskhand Agentic Workflow

Check items off as you go.

## Phase 1: Foundation
- [x] Define what an agentic workflow is and how it differs from RPA
- [x] Write `requirements.md` with functional and non-functional requirements
- [x] Choose a scope: a project and task management agent
- [x] Define the five tools and their inputs

## Phase 2: Agent core
- [x] Build the board state (team, capacity, tasks)
- [x] Build the planner that turns a goal into a tool-call queue
- [x] Build the executor that runs one tool and returns an observation
- [x] Build the re-planner that inserts corrective steps
- [x] Add a retry limit of three passes

## Phase 3: Scenarios
- [x] Launch: create and assign tasks, re-plan when an owner is full
- [x] Rebalance: measure, move lowest-priority work, verify
- [x] Report: summarize, flag overdue work, finish

## Phase 4: Interface
- [x] Goal input with one-click examples
- [x] Step log showing thought, action, and observation
- [x] Distinct style for re-planned steps
- [x] Live team capacity and task list
- [x] Run, Pause, Step once, and Reset controls
- [x] Keyboard focus, reduced motion, mobile layout

## Phase 5: Verify
- [ ] Run each example goal and confirm the acceptance criteria in `requirements.md`
- [ ] Test a custom goal and confirm it falls back to the launch flow
- [ ] Test Pause, Step once, and Reset mid-run
- [ ] Check the page at phone width

## Phase 6: Next steps
- [ ] Replace the rule-based planner with an LLM using tool calling
- [ ] Put the LLM call behind a backend endpoint (no keys in the browser)
- [ ] Connect the tools to the real task API and database
- [ ] Add auth so each user's agent only sees their projects
- [ ] Add a human approval step before the agent changes real data
- [ ] Record a short demo with narration and add it to the repository
