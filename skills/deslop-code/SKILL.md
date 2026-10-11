---
name: deslop-code
description: >-
  Use before writing or modifying code, during code review or cleanup, and before
  validating, finalizing, or handing off code. Prevent and remove AI code clutter:
  narrative comments, redundant guards, fallback scaffolding, type escapes,
  empty abstractions, and unreadable flow. Also use for deslop or de-AI requests.
  Prose and translation use deslop-writing.
metadata:
  version: "1.1.0"
---

# Deslop Code

Make changed code readable in the repository's style. Remove agent scaffolding while preserving supported behavior.

Before implementation or review, read the relevant sections of [patterns.md](references/patterns.md). Check the actual diff again at finalization.

## Establish scope

- Use neighboring code and relevant callers to establish repository style and contracts. Review, audit, and recommendation requests are read-only.
- Honor named paths or a supplied diff. Otherwise inspect changes from the actual merge base plus staged, unstaged, and relevant untracked source. Use the base established by PR/repository context. A branch's upstream may track the feature branch itself. If ambiguity affects scope, state the known range and ask. Outside Git, use the supplied material.
- Keep unrelated work intact. Generated code, vendored files, lockfiles, snapshots, and migration history require explicit inclusion. Adjacent callers and tests provide evidence without expanding edit scope.

## Make justified changes

For a semantic edit, establish its caller and input path, protected invariant, and replacement behavior.

- Remove comments that repeat syntax. Retain rationale, units, ordering constraints, bug/workaround context, licenses, public API documentation, and useful teaching comments.
- Remove guards only when the runtime data path proves redundancy. Retain necessary boundary validation for JSON, persisted records, plugin data, JavaScript callers, and external inputs; static types provide no runtime validation.
- Preserve failure semantics, caller-visible diagnostics, cancellation, resource cleanup, side-effect order, transactions, concurrency, security boundaries, and supported compatibility.
- Inline wrappers that contribute no concept, lifecycle, ownership, test seam, or invariant. Keep meaningful helpers with one caller. Choose readable flow and accurate types; retain checking and justified third-party typing workarounds.
- Keep tests of distinct rejection, empty-result, retry, and boundary behavior. Consolidate duplication within scope; use verification that can expose a real regression.
- Preserve public names, stored formats, error strings, and user-facing wording unless their change is authorized. Fix objective bugs in scope deliberately and report the behavior change.
- Check suspicious APIs against the installed version or authoritative source. Verify the real consumer contract independently of its mock.
- When evidence is missing, retain the construct and report the uncertainty. A no-change result is valid.

## Verify and finish

Run relevant existing checks and required repository checks. Preserve their coverage and strictness. Inspect the final diff for scope, lost behavior, type escapes, altered fallbacks, and new scaffolding.

Report meaningful changes, checks actually run, failed or unavailable checks, and unresolved uncertainty. Distinguish inspection findings from behavior verified by executed checks. Keep session notes in the report.
