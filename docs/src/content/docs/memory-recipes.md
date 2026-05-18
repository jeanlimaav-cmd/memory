---
title: Memory Recipes
description: Copyable prompts for specializing Memory without adding schemas, templates, or automatic capture.
---

Memory is the local, reviewable base layer. Recipes are the specialization
layer: copyable prompts that tell an agent what durable project knowledge to
save with the existing Memory commands.

Recipes do not add new object types, schemas, templates, plugins, or hidden
capture. They help the agent shape existing primitives such as `source`,
`synthesis`, `decision`, `constraint`, `fact`, `gotcha`, `workflow`, `question`,
`note`, and `concept` memories.

Use recipes when you want better project memory for a specific kind of work:
product planning, architecture, debugging, team workflow, source-backed notes,
or memory repair.

## Recipe format

Each recipe follows the same shape:

- When to use it: the situation where the recipe helps.
- Copyable prompt: the prompt to give the agent from the project root.
- Expected memory shape: the memory objects the agent should consider.
- What not to save: the boundaries that keep Memory useful.
- Verification command: the read-only or review command to run before finishing.

The routine write path is still:

```bash
memory remember --stdin
memory diff
```

Use the wiki path only when raw source identity matters:

```bash
memory wiki ingest --stdin
memory diff
```

## Product memory

Use this when the agent needs to clarify what the project is for, what it
already does, and what product questions remain open.

```text
Build or repair product memory for this repository.

Inspect the README, docs, product notes, visible app behavior, package
metadata, and current Memory. Save only durable product context future coding
agents should rely on: product intent, feature map, roadmap, user promise,
capability boundaries, and open product questions.

Prefer updating existing product, feature-map, roadmap, concept, or question
memory over creating duplicates. Do not invent features from weak signals.
Do not save a meeting transcript, task diary, or speculative idea as fact.

Use `memory remember --stdin`, then report the `memory diff` review path.
```

Expected memory shape:

- `synthesis` for product intent, feature map, or roadmap.
- `concept` for durable capabilities or domain concepts.
- `question` for unresolved product scope decisions.
- `source` when product context came from a stable file or user-stated source.

Verification command:

```bash
memory diff
```

## Architecture memory

Use this after architecture exploration, refactors, boundary decisions, or
design discussions that future agents should not rediscover.

```text
Build or repair architecture memory for this repository.

Inspect the relevant source, tests, docs, and current Memory. Save only durable
architecture context: service boundaries, module ownership, data flow,
invariants, decisions, constraints, known traps, and abandoned approaches that
future coding agents need before changing this area.

Prefer one precise memory per claim. Update, stale, or supersede old memory
when current code contradicts it. Do not save implementation play-by-play,
generic design advice, or guesses that are not supported by code, tests, docs,
or my stated context.

Use `memory remember --stdin`, then report whether `memory diff` is reviewable.
```

Expected memory shape:

- `architecture` or `synthesis` for compact area-level understanding.
- `decision` for chosen direction and rationale.
- `constraint` for invariants future work must preserve.
- `gotcha` for known failure modes.
- `question` for unresolved boundary decisions.

Verification command:

```bash
memory audit --json
memory diff
```

## Debugging memory

Use this after a hard bug, repeated failure, confusing behavior, or user
correction exposed knowledge future agents should have loaded earlier.

```text
Review Memory after this debugging session.

Use the bug symptoms, reproduction path, root cause, fix, tests, logs I shared,
current code, and loaded Memory as evidence. Save only reusable debugging
knowledge future agents should know: the failure mode, cause, fixed invariant,
regression risk, or correction to stale memory.

Update or supersede old memory if it caused the wrong assumption. Do not save
raw sensitive logs, temporary stack traces, a task diary, or "tests passed" as
memory.

Use `memory suggest --after-task "<task summary>" --json` if the save decision
is not obvious, then use `memory remember --stdin` only for durable knowledge.
Finish by reporting `memory diff`.
```

Expected memory shape:

- `gotcha` for repeated traps and failure modes.
- `fact` for precise debugging facts.
- `constraint` for invariants that prevent recurrence.
- `workflow` for a reusable reproduction or verification routine.
- `stale` or `supersede` actions for memory that misled the agent.

Verification command:

```bash
memory suggest --after-task "<task summary>" --json
memory diff
```

## Workflow memory

Use this when setup, release, migration, verification, maintenance, or support
steps should become a durable project runbook.

```text
Create or repair workflow memory for this repository.

Inspect package scripts, docs, CI config, release notes, agent guidance, and
current Memory. Save only project-specific procedures future agents should
repeat: setup, verification, release, migration, recovery, debugging,
maintenance, or support workflows.

Keep each workflow actionable and scoped. Include commands only when they are
stable project commands. Do not save generic tutorials, one-off task notes, or
temporary commands that only mattered for this branch.

Use `memory remember --stdin`, then report `memory diff`.
```

Expected memory shape:

- `workflow` for repeatable project procedures.
- `constraint` for workflow prerequisites or safety boundaries.
- `gotcha` for known traps in the procedure.
- `source` when the workflow is derived from stable docs or manifests.

Verification command:

```bash
memory audit --json
memory diff
```

## Source-backed wiki memory

Use this when a stable source should be preserved with provenance: docs,
issues, specs, design notes, research notes, meeting notes, transcripts, or
external references recorded by the agent.

```text
Ingest source-backed project memory.

Read the source I provide or point you to. Summarize the source as a Memory
`source` record with origin metadata, then save only maintained syntheses or
atomic claims future coding agents should retrieve later.

Link new memories back to the source. Do not copy the entire raw source into
Memory, save unsupported claims, or treat tentative notes as final decisions.

Use `memory wiki ingest --stdin` for source-backed saves, then run
`memory wiki lint --json` and report `memory diff`.
```

Expected memory shape:

- `source` with `origin` for raw-source identity.
- `synthesis` for maintained summaries.
- `decision`, `constraint`, `fact`, `gotcha`, `workflow`, `question`, `note`,
  or `concept` when a precise durable claim is useful.
- `derived_from`, `supports`, `summarizes`, or `documents` relations to the
  source.

Verification command:

```bash
memory wiki lint --json
memory diff
```

## Memory repair

Use this when Memory feels noisy, stale, contradictory, under-sourced, or too
broad after real work.

```text
Repair Memory after this task.

Run read-only Memory review commands first. Use current code, tests, docs, and
my corrections as higher-priority evidence than loaded Memory. Find stale,
duplicate, conflicting, weakly evidenced, overbroad, or task-diary-like memory.

Prefer updating, marking stale, superseding, or deleting existing memory over
creating a new object. Create new memory only when a durable gap remains. Do
not hide uncertainty; save an open question when the project needs one.

Use `memory audit --json` and `memory suggest --after-task "<task summary>"
--json`, then use `memory remember --stdin` only if a durable repair is needed.
Finish by reporting `memory diff`.
```

Expected memory shape:

- `updates` for memories that need sharper current wording.
- `stale` for obsolete claims.
- `supersede` for replacements that should stay connected to old memory.
- `question` for unresolved conflicts.
- New `gotcha`, `workflow`, `constraint`, or `synthesis` only when repair
  exposes a reusable gap.

Verification command:

```bash
memory audit --json
memory diff
```

## Boundaries

Recipes should make Memory more useful without changing the product contract.

- Use existing commands and object types.
- Keep Memory local, explicit, and reviewable.
- Save nothing when there is no durable future value.
- Keep agent setup guidance in [Agent recipes](/agent-recipes/).
- Keep source-provenance details in [Wiki workflow](/wiki-workflow/).
