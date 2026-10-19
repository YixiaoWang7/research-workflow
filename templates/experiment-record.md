# <Task> <label> (YYYY-MM-DD)

<!-- Copy to docs/experiments/<task>-<YYYY-MM-DD>-<label>.md. Once written, a record is immutable:
correct it only with a dated note. It must stay useful after the local files are gone. -->

**Kind:** reference | production | diagnostic | validation · **State:** complete | failed ·
**Machine:** <hostname, GPU>

## Summary

<What was run and why, and the conclusion, in two or three sentences.>

## Commands

```bash
<the exact commands, with repository-relative paths>
```

## Identity

| | |
| --- | --- |
| Code | `<commit>`; uncommitted files: <none, or the list> |
| Configuration | `<path>`, SHA-256 `<hash>` |
| Seeds and budget | <seed, updates or episodes, environment count> |

## Results

| Metric | Value | Samples | W&B |
| --- | --- | --- | --- |
| <name, and what it measures> | <value> | <count> | <run URL> |

## Artifacts

| Artifact | Path | SHA-256 |
| --- | --- | --- |
| <best checkpoint> | `<path>` | `<hash>` |

## Notes

<Validation evidence, failed or superseded attempts that matter, and limitations.>
