# aitaskcurrentbaddwork

## Purpose

This is the extra control layer for the AI work system.

The rule is simple:

> **A task is the unit of context.**
>
> Give an AI one bounded job, the minimum context needed to do that job, a clear output, and a hard stop.
>
> Do not make every AI read the whole project.

This system exists to keep the main project clean while still allowing many AIs to work in parallel.

## Architecture

```
HUMAN
  |
  v
CHATGPT / ORCHESTRATOR
  |
  +--> TASK BOARD (aitaskcurrentbaddwork)
  |       |
  |       +--> one task
  |       +--> one owner
  |       +--> one bounded context
  |       +--> one expected output
  |       +--> one stop condition
  |       +--> one evidence requirement
  |
  +--> AGENT-SPECIFIC PROMPT
  |
  +--> AGENT
  |       Claude   = implementation
  |       Grok     = attack / falsification
  |       Manus    = forensics / mapping
  |       Replit   = runtime / environment
  |       Notion   = knowledge / task memory
  |       ChatGPT  = orchestration / reconciliation
  |
  +--> EVIDENCE / HANDOFF
  |
  +--> TASK RESULT
  |
  +--> ORCHESTRATOR DECISION
          |
          +--> next task
          +--> split task
          +--> retry with smaller context
          +--> stop
          +--> human gate
```

## Three storage layers

### 1. MAIN BAD-D REPO = protected production control plane

The main repository contains authoritative production contracts, safety rules, evidence gates, current project state, and deliberate holds.

**Do not delete a hold merely because it is inactive.**

A hold is cheap durable memory.

If a hold is no longer active, mark it superseded/deprecated rather than rewriting history.

### 2. badDdump = experimental/control-tool workspace

This repository contains tools and experiments that help control, compress, route, test, inspect, or coordinate AI work.

It is deliberately separate from production.

Do not silently promote code or decisions from this repository into BAD-D production.

### 3. aitaskcurrentbaddwork = task control layer

The task board is the working queue.

It should contain small jobs, not giant project descriptions.

The board answers:

- What needs doing?
- Why does it exist?
- Who owns it?
- What is the smallest useful context?
- What output is required?
- What evidence proves completion?
- When must the agent stop?
- What happens next?

It should NOT become another giant project encyclopedia.

## Visibility rule

Every piece of information gets a visibility class:

- PUBLIC: safe for public repositories, public issue text, public documentation.
- INTERNAL: useful to the project but not intended for public publication.
- PRIVATE: personal/account/workspace information.
- SECRET: credentials, tokens, private keys, authentication material, private URLs, or equivalent.

Default is **INTERNAL** unless deliberately classified otherwise.

Never put SECRET material into a task, prompt, public repository, issue, log, or evidence file.

Never copy private production data into the public experimental repository.

A task may contain a reference to protected material without copying the protected material itself.

Preferred pattern:

```
READ: main-repo/.github/AI_WORKFLOW_CONTRACT.md
DO NOT COPY CONTENT INTO THIS TASK.
USE ONLY THE RULES REQUIRED FOR THIS JOB.
```

## Task contract

Every task must have these fields:

1. TASK-ID
2. STATUS
3. PRIORITY
4. AGENT
5. WORK TYPE
6. REPO
7. BRANCH
8. PARENT TASK
9. MISSION
10. IN SCOPE
11. OUT OF SCOPE
12. CONTEXT BUDGET
13. INPUTS
14. EXPECTED OUTPUT
15. EVIDENCE REQUIRED
16. STOP CONDITION
17. DEPENDENCIES
18. HUMAN GATE
19. NEXT ACTION

If one of these is unknown, record UNKNOWN.

Do not invent missing information.

## Task size rule

A task is too large when the agent needs to understand several unrelated systems before it can start.

Split it.

Good:

- "Determine whether toggle OFF still allows autonomous pipeline enqueue."
- "Run the 15-track RAM test and report peak pressure."
- "Attack the proposed scope fingerprint with five counterexamples."
- "Implement only the confirmed OFF-state gate."

Bad:

- "Fix BAD-D."
- "Improve the Testing Deck."
- "Make the whole AI system smarter."
- "Review the entire repository."

## Context budget

Each task has a context budget.

The budget is a control limit, not a target.

Recommended starting levels:

- 1 = tiny lookup / exact file check
- 2 = one file or one narrow question
- 3 = one subsystem
- 4 = subsystem + relevant contract
- 5 = cross-agent reconciliation
- 6 = major architectural decision
- 7+ = exceptional; split unless there is a strong reason

Free AI usage should normally use levels 1–4.

Only give level 5+ when the task genuinely requires cross-system reasoning.

## The context ladder

Agents receive context in this order:

### Level 0 — task card

Only:
- task ID
- mission
- expected output
- stop condition

### Level 1 — exact evidence

Add:
- exact file
- exact function
- exact commit
- exact reproduction
- relevant test result

### Level 2 — local contract

Add only the specific project rule needed.

### Level 3 — subsystem context

Add the smallest surrounding system needed to understand the evidence.

### Level 4 — cross-system context

Use only when the task cannot be solved locally.

### Level 5 — orchestration context

Only ChatGPT/reconciliation jobs normally need this.

**Never start at Level 5 just because it is available.**

## Context handoff format

Agents should receive:

```
TASK-ID:
ROLE:
MISSION:
IN SCOPE:
OUT OF SCOPE:
REPO:
BRANCH:
KNOWN FACTS:
INPUT EVIDENCE:
EXPECTED OUTPUT:
EVIDENCE REQUIRED:
STOP IF:
CONTEXT BUDGET:
DEPENDENCIES:
```

Nothing else unless required.

## Result handoff format

Agents return:

```
TASK-ID:
STATUS: DONE | BLOCKED | FAILED | STOPPED
ROLE:
BRANCH:
COMMIT:
FILES:
QUESTION ANSWER:
FACTS:
TESTS:
RESULTS:
UNVERIFIED:
RISKS:
SCOPE CHANGES: NONE | DESCRIBED
NEXT SAFE ACTION:
```

The agent does not decide the next project task unless explicitly assigned that authority.

## Agent automation plan

### ChatGPT — ORCHESTRATOR

Input:
- task board
- current authoritative state
- completed handoffs
- human direction

Job:
- create/split tasks
- choose the cheapest discriminating next test
- assign agents
- keep context small
- reconcile conflicting evidence
- decide whether a task can proceed
- stop when human input is required

Do not:
- blindly implement because an agent suggested it
- treat a report as proof
- send the whole project to every agent

Default context: Level 3–5.

### Claude — IMPLEMENTATION ENGINE

Input:
- one implementation task
- exact files
- exact acceptance test
- relevant contract only

Job:
- implement the smallest confirmed change
- test it
- report exact diff/result

Do not:
- redesign the system unless task explicitly says so
- expand scope
- fix unrelated issues
- infer missing requirements

Default context: Level 2–4.

### Grok — ADVERSARIAL / FALSIFICATION

Input:
- proposed claim
- evidence
- proposed implementation/design

Job:
- attack the claim
- find counterexamples
- identify hidden assumptions
- propose the cheapest test that could disprove it

Do not:
- become the implementation owner unless explicitly assigned
- rewrite the project
- turn speculation into fact

Default context: Level 2–3.

### Manus — FORENSICS / SYSTEM MAPPING

Input:
- exact artifact
- exact question

Job:
- trace code/data flow
- identify coverage gaps
- distinguish static facts from runtime facts
- produce evidence-backed maps

Do not:
- silently implement production changes
- treat static analysis as runtime proof

Default context: Level 2–4.

### Replit — RUNTIME / WORKING ENVIRONMENT

Input:
- exact runtime test
- exact branch/build
- success/failure criteria

Job:
- run the application
- reproduce the bug
- collect runtime evidence
- test resource behavior
- verify implementation behavior

Do not:
- redesign the architecture during a runtime test
- report "fixed" without the specified test
- promote changes automatically

Default context: Level 1–3.

### Notion — KNOWLEDGE / TASK MEMORY

Input:
- completed task results
- decisions
- stable procedures

Job:
- store readable knowledge
- maintain the task board
- preserve durable explanations
- expose current queues

Do not:
- become the source of truth for production code
- silently change engineering decisions

Default context: Level 1–3.

### Human — AUTHORITY / FINAL GATE

Human input is required for:
- ambiguous scope
- destructive operations
- production promotion
- irreversible changes
- conflicting evidence that cannot be resolved
- privacy/security boundary decisions
- stopping/continuing expensive work when the user has not specified the preference

## Queue algorithm

The orchestrator should select work in this order:

1. P0 safety / correctness blocker
2. Cheapest test that can eliminate a major uncertainty
3. Blockers for already-approved implementation
4. Small implementation tasks
5. Adversarial checks
6. Documentation / cleanup
7. Future ideas

Never start five implementation tasks when one small test can tell us which four are unnecessary.

## Parallelism rule

Maximum default active lanes:

- 1 implementation lane
- 1 adversarial/research lane
- 1 runtime/expensive-test lane

More parallelism requires an explicit reason.

The goal is not maximum simultaneous AI activity.

The goal is maximum useful verified progress per unit of context, time, and money.

## Free-use optimization

For free/limited AI:

- prefer narrow tasks
- reuse exact evidence rather than repeating prose
- send file paths and line/function targets
- do not resend entire reports
- store results once
- pass references instead of copying large documents
- stop failed approaches quickly
- use cheap static tests before expensive runtime tests
- use one agent to investigate and another to attack only when that separation adds information

### Context compression rule

If a task prompt becomes long:

1. remove repeated prose
2. remove solved history
3. remove unrelated agent reports
4. keep exact identifiers
5. keep constraints
6. keep acceptance criteria
7. keep contradictions
8. keep unknowns that affect the result
9. keep evidence references
10. stop compression if meaning could change

This is where the dumb-ass context stripper belongs.

It is a tool for reducing context, not a second project manager.

## Loss-of-scope limiter

Before and after compression, compare:

- mission
- non-goals
- hard constraints
- safety gates
- evidence requirements
- acceptance criteria
- protected identifiers
- explicit human decisions
- unresolved contradictions
- important UNKNOWN states

If any protected item disappears, changes meaning, becomes ambiguous, or turns UNKNOWN into FACT:

```
SCOPE_LOSS -> STOP
```

Do not guess.

## State machine

```
NOT STARTED
    |
    v
READY
    |
    v
IN PROGRESS
    |
    +--> BLOCKED
    |
    +--> STOPPED
    |
    v
RESULT READY
    |
    v
VERIFICATION
    |
    +--> FAILED -> new/smaller task
    |
    v
DONE
    |
    v
HANDOFF
    |
    v
ORCHESTRATOR RECONCILIATION
```

A task is not DONE merely because an agent produced text.

DONE requires the task's specified evidence.

## Automation boundaries

Automation may:

- create tasks from approved templates
- assign a predefined agent
- add required context references
- update task status
- request a handoff
- notify the next owner
- create follow-up tasks from explicit rules
- archive completed task summaries

Automation must not:

- promote production code
- bypass evidence gates
- rewrite human decisions
- delete holds
- expose private/secret information
- silently expand scope
- convert hypotheses into facts
- mark a task verified without its required evidence

## Public/private workflow

Public experimental repository:
- reusable tools
- generic control logic
- synthetic fixtures
- public documentation
- non-sensitive test harnesses

Private/main project:
- production code
- private fixtures
- real library data
- internal architecture
- private evidence
- credentials
- deployment details
- human-only decisions

When uncertain, keep it private.

## Holds

Holds are intentional.

A hold means:

> Do not proceed past this boundary until its condition is satisfied.

Do not delete old holds simply to make the workspace look clean.

Instead:

- ACTIVE
- SUPERSEDED
- RELEASED
- DEPRECATED

This preserves why a boundary existed.

## Minimal task example

```
TASK-ID: AI-001
STATUS: READY
PRIORITY: P0
AGENT: Replit
WORK TYPE: Runtime Test
REPO: bad_d_meomory
BRANCH: current test branch

MISSION:
Prove whether Testing Deck OFF still permits the autonomous pipeline to enqueue work.

IN SCOPE:
One OFF-state upload test.

OUT OF SCOPE:
Any code modification.

CONTEXT BUDGET:
2

EXPECTED OUTPUT:
Observed runtime result with exact evidence.

EVIDENCE REQUIRED:
Console/runtime trace showing whether enqueue occurred.

STOP CONDITION:
Stop after the single reproduction is complete.

HUMAN GATE:
No
```

## Why this prevents the project becoming massive

The project can still become large.

The **individual job does not**.

That distinction is the whole system.

Large project:
- many small tasks
- many small evidence records
- many small handoffs

Not:
- one giant prompt
- one giant agent session
- one giant context dump
- one giant "fix everything" task

The task board becomes the traffic controller.

The repositories remain the memory.

The agents become specialists.

ChatGPT remains the orchestrator.

The human remains the authority.

## Initial build order

1. Create the task board.
2. Create the task-card template.
3. Create the agent-specific prompt templates.
4. Create the context-budget rules.
5. Create the public/private classification.
6. Connect task completion to evidence/handoff.
7. Connect task handoff to the next task.
8. Add the loss-of-scope limiter.
9. Test with synthetic tasks.
10. Only then automate task creation/routing.
11. Only after that consider deeper agent-to-agent automation.

## Hard rule

**Do not automate a workflow that has not first worked manually with bounded tasks.**

Automation should remove repetitive movement.

It must not hide the reasoning.

## Definition of success

The system succeeds when:

- no agent needs the whole project to perform a small job
- tasks can be handed to different AIs without rewriting the entire project history
- every task has a clear stop condition
- completed work produces evidence
- holds survive cleanup
- public/private boundaries are explicit
- failed work does not contaminate unrelated tasks
- context stays small enough for limited/free AI usage
- the main production repository remains clean
- the experimental control repository remains separate
- the human can see what every agent is doing without reading every agent's full context
