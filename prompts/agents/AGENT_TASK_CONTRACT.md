# AI Task Control — Agent Handoff Contract

Read this file only when a task explicitly points you here. Do not load the whole control system for ordinary work.

## Rule
One task = one bounded job = one owner = one expected output = one stop condition.

Do not solve the whole project.

## Required task input
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

## Required result
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

## Hard stops
STOP if:
- the task requires unrelated work;
- requirements are ambiguous;
- protected production boundaries would be crossed;
- evidence is missing for a claim that must be verified;
- you would need to guess;
- the task would require substantially more context than its budget;
- a proposed compression changes mission, constraints, safety gates, acceptance criteria, protected identifiers, unresolved contradictions, or UNKNOWN states.

Report the stop. Do not silently continue.

## Context discipline
Prefer references to large copied text.
Read the exact files/functions needed.
Do not ask for the entire project unless the task genuinely requires it.
Do not turn a hypothesis into a fact.
Do not turn a suggestion into a requirement.

## Visibility
PUBLIC = safe to publish.
INTERNAL = project information not intended for public publication.
PRIVATE = private workspace/project information.
SECRET = credentials, tokens, keys, authentication material.

Never place SECRET information in a task, prompt, repository, issue, log, or handoff.
When uncertain, keep information private.

## Holds
Do not delete holds merely because they are old.
Use ACTIVE, SUPERSEDED, RELEASED, or DEPRECATED.
A hold is durable scope memory.

## Agent boundaries
Claude: implement only confirmed bounded changes.
Grok: attack claims/designs and find counterexamples.
Manus: map/trace and separate FACT from UNKNOWN.
Replit: run bounded runtime tests and collect runtime evidence.
ChatGPT: orchestrate, reconcile evidence, split work, and decide next safe task.
Notion: store task state and durable readable knowledge.
Human: final authority for ambiguous, destructive, irreversible, privacy, security, and production-promotion decisions.

## Default parallelism
1 implementation lane + 1 adversarial/research lane + 1 runtime/expensive-test lane.

More requires an explicit reason.

## Completion
Text output alone is not proof.
A task is DONE only when its required evidence exists.
