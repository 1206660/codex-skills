---
name: milestone-iteration-loop
description: Use when the user wants autonomous repeated iteration toward a concrete milestone instead of a single pass. Triggers on requests like keep iterating until X passes, don't stop after one round, continue running batches until the milestone is reached, or continuously test-train-fix until a target level, benchmark, or gate is achieved.
---

# Milestone Iteration Loop

Use this skill when the task is not "do one implementation pass" but "keep cycling until a milestone is achieved."

Typical examples:
- "Keep iterating until level01 passes."
- "Don't stop after one batch, keep testing and fixing."
- "Run continuous train/test/analyze loops until the gate passes."
- "Keep optimizing until success rate reaches the target."

## Outcome

Drive a closed loop:

`baseline -> hypothesis -> edit -> verify -> compare -> retain or revert -> next hypothesis`

Do not stop after a single failed batch. Stop only when:
- The milestone is reached and verified.
- A real hard blocker is hit.
- The user explicitly redirects or stops the work.

## Required Inputs

Before entering the loop, infer or record:
- Milestone: the concrete end condition.
- Primary metric: what counts as progress.
- Verification command or run method.
- Best retained baseline: the last known good state.

If the user did not spell these out, infer them from project context and logs.

## Default Loop

1. Reconfirm the current best retained baseline.
2. Identify the narrowest current blocker from logs, tests, or metrics.
3. Pick one small hypothesis that targets that blocker.
4. Implement only that hypothesis.
5. Run the real verification path, preferably as a batch rather than a single sample.
6. Compare the result to the best retained baseline.
7. Keep the change only if it shows verified positive signal.
8. Revert no-signal or negative-signal changes immediately.
9. Start the next hypothesis without waiting for user confirmation.

## Validation Rules

- Prefer repeated runs over one-off wins.
- Compare against the last verified baseline, not intuition.
- Count stage reach, success rate, failure mode, and regression spread.
- Treat "deeper but less stable" as provisional, not automatically positive.
- A change is positive only if the signal is reproducible or clearly stronger than noise.

## State Discipline

Maintain a lightweight iteration ledger for the active milestone. Include:
- Current milestone
- Best retained state
- Latest blocker
- Positive experiments
- Reverted experiments
- Next hypothesis

Use the template in `references/ledger-template.md`.

## Change Discipline

- Keep each experiment narrow.
- Do not stack multiple unrelated hypotheses in one batch.
- If a change hurts throughput to earlier stages, revert it even if it helps a later stage once.
- Preserve the best retained state in the working tree between loops.

## Delegation

For non-trivial loops, use at least one subagent each cycle for sidecar work such as:
- log clustering
- batch result comparison
- hypothesis ranking
- searching historical regressions

Do not hand off the blocking edit itself if you need the answer immediately.

## Publication Rule

If the project already has a rule for publishing verified progress, follow it on every positive step.

Default publication behavior:
- Stage the relevant code changes.
- Stage only the logs that validate this specific improvement.
- Commit and push only after the gain is verified.

Do not publish negative or noisy experiments as progress.

## Hard Blockers

Stop only for issues like:
- Build or runtime can no longer be repaired with local context.
- Required tool or environment is missing and cannot be discovered locally.
- The next step would risk destructive or directionally expensive changes without enough evidence.

Normal failed batches are not blockers. They are loop input.

## RTTSimulator Pattern

For the current project, a good loop usually looks like:

`pick one level01 blocker -> patch one narrow behavior -> build -> run 6 headless workers -> compare stage reach and failure pattern -> keep or revert -> continue`

Use milestone language explicitly in updates and notes, for example:
- `Milestone: level01 autonomous clear`
- `Current blocker: brazier_north -> doc_orders`
- `Best retained state: last verified batch with deepest stable progress`

