# badDdump

`badDdump` is an experimental workspace for documenting and exploring ways to keep AI work bounded, coordinated, and separate from production. The guide describes the intended architecture, task contract, context ladder, handoffs, queue and state machine, automation boundaries, and holds.

## Start here

- [Primary guide: `aitaskcurrentbaddwork.md`](aitaskcurrentbaddwork.md) — project purpose, architecture, task sizing and context rules, handoff formats, workflow boundaries, and examples.
- [Shared agent task contract](prompts/agents/AGENT_TASK_CONTRACT.md) — required task inputs and results, hard stops, visibility rules, and agent boundaries.

## Role prompt templates

- [ChatGPT orchestrator](prompts/agents/CHATGPT_ORCHESTRATOR_TEMPLATE.md) — choose and define the smallest useful next task.
- [Claude implementation](prompts/agents/CLAUDE_TASK_TEMPLATE.md) — implement one confirmed, bounded change and run its acceptance test.
- [Grok adversarial review](prompts/agents/GROK_TASK_TEMPLATE.md) — challenge a claim or design and look for counterexamples.
- [Manus forensic mapping](prompts/agents/MANUS_TASK_TEMPLATE.md) — trace a specified artifact and distinguish static from runtime evidence.
- [Replit runtime testing](prompts/agents/REPLIT_TASK_TEMPLATE.md) — run a bounded test and capture observed runtime evidence.

## Scope

The tracked material at this baseline is a guide and Markdown prompt templates. They describe intended practices; this README does not claim that executable workflow or runtime tooling is present, or that task claims and boundaries are automatically enforced.
