# Artifacts And Exit Codes

Keep artifact ownership and exit semantics explicit.

## Per-run artifact layout

Prefer a structure like:

```text
artifacts/
  test-runs/
    <run-name>/
      worker-01/
        logs/
        state/
        outputs/
      worker-02/
        logs/
        state/
        outputs/
```

Adjust names to the repository, but keep worker isolation intact.

## Store separately

- Godot log output
- stdout capture
- stderr capture
- summary or report files
- worker-local state
- generated datasets or result payloads

## Exit-code rules

Return failure when:

- the process returns a non-zero exit code
- required summary files are missing
- pass thresholds are not met
- an expected artifact directory is empty

Return success only when:

- the process completes
- the required artifacts exist
- the validation rule for that run mode passes

## Guardrails

- do not let multiple workers share a mutable state file
- do not overwrite a previous run unless the command explicitly says so
- do not treat any single log line as success without checking the full run result
