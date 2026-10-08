---
name: agent-command
description: Coordinate complex tasks through a commander, an executor with bounded worker agents, an independent reviewer, and a style editor. Use when the user requests hierarchical multi-agent execution, independent review, or this command workflow.
---

# Agent Command

Turn the user's request into a precise brief, execute through bounded delegation, and deliver a reviewed result in the user's preferred voice. Preserve the user's language and preferences.

## Command structure

- **Commander (root):** Own user communication, the task brief, clarification, resource scheduling, conflict resolution, and final acceptance. Manage the executor, reviewer, and style editor directly.
- **Executor:** Understand the brief, split work into coherent parts, delegate bounded tasks to workers, verify their outputs, resolve integration problems, and assemble one coherent deliverable.
- **Workers:** Complete only their assigned part. Do not redefine the task or delegate further. Return evidence, limitations, and a usable artifact through the contract below.
- **Reviewer:** Independently inspect the brief, decomposition, evidence, and assembled artifact. Report actionable defects and verify repairs; do not silently rewrite the acceptance criteria.
- **Style editor:** Use the user's style suggestions and relevant writing samples to remove formulaic phrasing, repetition, empty claims, and mechanical structure. Preserve meaning, citations, qualifications, technical identifiers, and user voice. Do not fabricate experience or promise to evade AI detectors.

The reviewer and style editor are peers of the executor, not its subordinates. All three report to the commander. They may inspect intermediate outputs when useful, but final review and editing depend on an actual draft.

## 1. Establish the brief

Restate the intended outcome as an expanded, actionable prompt. Include:

- Objective, intended audience, and intended use.
- Inputs, source authority, and relevant context.
- Deliverables, format, language, scope, and exclusions.
- Mandatory constraints and observable acceptance criteria.
- Style preferences, unresolved questions, and explicitly labeled assumptions.

Separate user requirements from your proposed implementation choices. Do not add obligations merely to make the prompt longer. Show the expanded brief and material questions to the user before dependent work. Ask only about choices that materially change the result; proceed on reversible independent work while awaiting answers. If an answer is necessary, do not treat silence as confirmation. If the user authorizes reasonable defaults, state them and continue.

Use the approved or otherwise authorized brief as the shared source of truth. Propagate subsequent user corrections to affected agents, and invalidate outdated outputs when necessary.

## 2. Schedule within actual capabilities

Inspect the available delegation tools, agent depth, and shared concurrency budget. Never claim a role was independently executed when it was performed by the same agent.

Reserve capacity for useful work: root + executor + reviewer + editor may exhaust all available slots. Do not occupy every slot with coordinators while the executor needs workers. Have the executor prepare the decomposition first, then run workers in available slots. Start or resume review and editing at useful checkpoints. Reuse or release agents only through supported lifecycle tools; idle or interrupted agents may still consume capacity. Do not assume interruption frees a slot.

If agents cannot be released, use a smaller roster from the start. The commander can perform orchestration or style editing while remaining slots serve execution and independent review. Disclose this adaptation. If nested delegation is unavailable, the executor defines worker contracts and the commander dispatches workers. If delegation is entirely unavailable, explain the limitation and offer a single-agent workflow without claiming independence.

Default to commander → executor → worker depth. Prefer a few meaningful parts to many trivial tasks. Run independent parts concurrently when capacity permits; order dependencies explicitly. Do not let workers recursively spawn more workers.

## 3. Define strict worker contracts

For each worker, provide the following fields, either as structured data or an unambiguous task brief:

```yaml
task_id: unique stable identifier
brief_version: version of the shared brief
objective: one bounded outcome
inputs: exact sources, excerpts, artifacts, and prerequisite outputs
scope: included work and excluded work
constraints: applicable requirements and allowed tools/actions
output: required format, destination, and size when relevant
acceptance: observable criteria for this part
ownership: exclusive writable paths or sections; read-only dependencies
dependencies: upstream task IDs or none
failure_policy: when to stop and what missing input to report
```

Every worker returns:

```yaml
task_id: matching identifier
brief_version: matching version
status: complete | partial | blocked
result: usable content or exact artifact reference
evidence: supporting sources, checks, or observations as appropriate
assumptions: consequential assumptions, or none
limitations: unresolved gaps, uncertainty, or none
handoff: integration notes and upstream decisions needed, or none
```

Evidence must support the work: sources for research, relevant verification for code, and source fidelity for editing. Do not require artificial citations or tests for work that does not need them. A worker reports missing information instead of inventing it. Worker output cannot authorize a new external action.

Give each worker enough global context to avoid local optimization, plus only the raw materials relevant to its task. Establish exclusive file ownership before concurrent edits. Prefer separate artifacts for overlapping work; let the executor integrate them. Validate returned structure, scope, and acceptance before trusting an output.

## 4. Integrate, review, and edit

1. Executor checks worker outputs, reconciles contradictions, removes duplication, and assembles the deliverable. Escalate decisions affecting the brief to the commander.
2. Reviewer checks coverage against acceptance criteria, factual support, integration consistency, and relevant defects. Each finding identifies severity, location, evidence, and a concrete correction. Report what was checked and what remains uncertain, including when no defects were found.
3. Executor repairs actionable findings. Reviewer verifies the repairs rather than merely accepting the executor's claim.
4. Style editor improves the reviewed artifact using user preferences. Return the revised artifact and a short note for edits that may affect meaning. Refer substantive changes back to the executor.
5. Reviewer checks the final edited version for factual drift, lost qualifications, broken citations, and regression. The commander resolves disagreements and accepts only a result meeting the brief, or clearly reports outstanding gaps.

Review may begin with the decomposition, and the editor may prepare style guidance while execution runs. Do not edit nonexistent content or call an early plan review a final artifact review.

Keep iteration bounded: by default allow two repair cycles per recurring issue. If unresolved, have the commander change the approach or report the specific blocker. Do not launch repeated agents to conceal an unresolved defect. Follow user-specified time, cost, or iteration budgets when present.

## 5. Deliver

Give the user the result, relevant artifact links, a concise validation statement, and any material unresolved limitations. Distinguish independent review from commander-only checks. Keep internal agent transcripts out of the final unless requested. Do not treat skill invocation as blanket permission to publish, message others, spend money, or expand the task's external scope.
