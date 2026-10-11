# Code patterns

Use patterns as prompts to inspect readability, maintenance cost, and contracts. Change a construct when that evidence justifies the replacement; keep code whose purpose or behavior remains necessary.

Read the relevant group: [comments](#comments), [validation and errors](#validation-and-errors), [types and flow](#types-and-flow), or [tests](#tests).

## Comments

### 1. Syntax narration

Remove comments such as `// Initialize the list`, `// Loop through tasks`, `// Check if valid`, `// Return the result`, and JSDoc that repeats a typed signature. Retain explanations of non-obvious input contracts, units, ordering, and externally visible errors.

```ts
// Calculate the sum of the estimates
const total = tasks.reduce((sum, task) => sum + task.estimateHours, 0);
```

The declaration explains the calculation. A comment explaining why cancelled tasks remain billable would add a business rule and should stay.

### 2. Step comments and banners

Replace `// Step 1: Validate`, `// Step 2: Process`, repeated "Helper Functions" banners, and statement-by-statement commentary with clear names and flow. Preserve navigation markers such as Swift `MARK` and tutorial comments that explain the concept being taught.

### 3. Session history

Remove session notes such as "As requested", "per reviewer feedback", "now handled here", "old approach removed", "fixes the previous issue", and review-agent names. For a necessary workaround, state the invariant or bug and retain a useful issue link alongside the reason.

### 4. Unsupported comment claims

Verify claims such as "Ensures thread safety", "guarantees cleanup", "robust error handling", and "supports all formats" against the implementation. Correct stale claims and remove empty praise. Change behavior only within the authorized task. Keep accurate API documentation, licenses, and required tool annotations.

### 5. Template residue

Remove stale `TODO: add your logic`, unused example branches, commented-out experiments, debug prints, and stub success results. Implement missing requested behavior within the authorized task. Retain tracked TODOs, intentionally abstract methods, and purposeful diagnostic logs.

## Validation and errors

### 6. Repeated validation

```ts
const phases = project.phases.map(phase => ({ ...phase, selected: false }));
return Array.isArray(phases) ? phases : [];
```

The local `map` result is already an array. Remove the redundant check. TypeScript's `Phase[]` annotation leaves deserialized input unvalidated at runtime. Preserve boundary validation, nullable API handling, persisted-version checks, and supported plugin contracts.

### 7. Optional chaining and defaults

For `requiredUser?.id`, `items ?? []`, `value || defaultValue`, and fallback chains, establish whether absence is valid, invalid, or impossible. Preserve valid empty, zero, and false values. `x ?? undefined` converts `null` to `undefined`; optional chaining can skip side effects. Resolve those behaviors before simplifying.

### 8. Errors returned as success

`catch { return []; }`, `catch { return true; }`, or `catch { return cachedValue; }` can turn a backend error into an empty screen, fake success, or stale data. Establish the recovery contract. Preserve documented offline/cache fallbacks and error-to-domain translation. Report a required outcome change as a bug fix and verify its behavior.

### 9. Rethrowing and duplicate logs

```ts
try {
  return calculateEstimate(tasks);
} catch (error) {
  throw error;
}
```

Remove this wrapper when it provides no cleanup, transformation, or other contract. Consolidate duplicate logs while preserving intentional observability, `finally`, contextual error causes, cancellation, and resource release. In an async `try`, `return await` lets the catch handle promise rejection. Awaiting a return from `try` or `catch` delays `finally` until the promise settles. Preserve these behaviors.

### 10. Speculative compatibility

Investigate nested "try current API, try legacy API, try manual implementation" paths, guessed environment probes, unlimited retries, and timeouts copied without a requirement. Check callers, versions, and support promises before removing obsolete internal variants. Preserve persisted formats and supported external consumers, including those absent from local search results.

### 11. Repeated repair layers

Accumulated fixes can leave several guards, wrappers, and recovery paths protecting the same state. Trace the complete path from input to result and consolidate redundant layers. Preserve the distinct responsibilities of input validation, authorization, and transaction rollback.

## Types and flow

### 12. Type escapes

Investigate `as any`, `as unknown as T`, `try!`, force unwraps, `type: ignore`, and `ts-ignore`. Repair the data shape, narrow at the appropriate boundary, or retain the repository's documented third-party typing workaround. Preserve checking when replacing an escape: implicit `any`, unchecked assertions, and disabled rules carry the same risk. Keep explained `ts-expect-error` uses in compile-time negative tests.

### 13. Forwarding helpers

```ts
const retrievePhaseTitleValue = (phase: Phase) => phase.title;
const title = retrievePhaseTitleValue(phase);
```

Use direct access here. Keep one-use helpers such as `releaseLease`, `parseTransportResponse`, or `roundToBillingIncrement` when they name a boundary or business rule. Decide by the helper's contribution, including ownership and test seams.

### 14. Unused abstraction

Simplify factories with one variant, strategies with one supported mode, classes wrapping a function, config maps for constants, and files/barrels created "for extensibility" when they add no current value. Reuse the direct existing pattern. Preserve dependency injection, lifecycle ownership, public extension points, and deliberate architecture; introduce a replacement abstraction only when it contributes a needed concept.

### 15. Dense expressions

```ts
const label = task.done ? "Done" : task.blocked ? "Blocked" : task.started ? "Active" : "Queued";
```

Use explicit branches or the established local form for multiple conditions. Split chains where names reveal distinct domain steps. Keep clear short ternaries and transformations. Check callback order, repeated passes, allocation costs, and lazy iteration before rearranging chains.

### 16. Nested flow and boolean arguments

An `if` inside an `if` inside a loop can obscure the operation. Use guard clauses when they clarify the flow and preserve cleanup and side-effect order. Investigate boolean arguments such as `processTask(task, true, false)` and redundant `x === true ? true : false`. Preserve intentional coercion, tri-state values, and API signatures. Clarify a single call with existing naming or local flow before introducing an options framework.

### 17. Mirrored state and speculative optimization

Mirrored derived state, effects that recompute other state, caches for cheap calculations, and pervasive memoization/callback wrappers can create stale values and lifecycle bugs. Use existing derived-data patterns. Preserve user edits, external synchronization, stable identity contracts, measured expensive work, and concurrency controls.

### 18. Elaborate names

Replace unclear `processedTaskDataArray`, `getCurrentSelectedPhaseIdentifierValue`, and needless `Manager`/`Helper`/`Factory` suffixes with concise domain names. Follow the repository's conventions for prefixes and casing. Preserve public identifiers, units, and important distinctions; spell out names when abbreviations obscure them.

## Tests

### 19. Mocks that prove themselves

A mock returns `42` and the test asserts the mocked function returns `42`. A fake backend implements the client's guessed behavior. Test the consumer through an existing public seam against an independently specified result. Keep mocks that isolate a real dependency while exercising consumer behavior.

### 20. Tests that copy the implementation

Expected values computed by the implementation's algorithm, regex checks for exact source lines, repeated config literals, and giant snapshots can miss regressions while duplicating maintenance. Establish what failure each test can expose. Preserve contract values, intentional UI-copy tests, schema invariants, useful snapshots, and distinct failure cases when consolidating coverage.

### 21. Invented APIs hidden by mocks

An installed client exposes `loadPhase`, but the code and its mock agree on an imaginary `fetchPhase`. Inspect exports, types, and documentation for the installed library and version. Confirm the actual API before correcting the consumer, then verify the real seam. A single text search may miss an export.
