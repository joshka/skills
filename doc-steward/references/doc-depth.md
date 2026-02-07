# Documentation Depth Guide

Use this guide for deep documentation passes in greenfield and docstring workflows.

## Goal

Produce docs that help maintainers make safe changes quickly. Favor more depth over less, but avoid
low-value commentary.

## Quality Floor

- Docs must add information not obvious from names/signatures.
- Explain contracts, constraints, and intent before implementation mechanics.
- If a claim is not provable from code/tests/usages, mark it `Unknown`.

## Depth Levels

Use these levels to decide required detail:

1. `L0` (insufficient): restates symbol name/signature only.
1. `L1` (basic): purpose plus one concrete contract detail.
1. `L2` (good): purpose, assumptions, side effects, and key constraints.
1. `L3` (strong): L2 plus edge/failure behavior and ordering/invariant rationale.

Target by pass:

1. `quick`: at least `L1` for key paths; `L2` for non-trivial touched code.
1. `full`: at least `L2` for all in-scope artifacts; `L3` for orchestrators/risky paths.

## Complexity Triggers

Require deeper docs when any of these are true:

1. Function is an orchestration or dispatch hub (many branches/modes).
1. Function mutates shared state or controls ordering of side effects.
1. Function executes subprocesses, I/O, rendering, or external calls.
1. Function has fallback behavior, defaults, or recovery paths.
1. Function includes safety gates, confirmation, or failure mapping.

## Module Depth

For each module in scope, document:

1. Purpose and boundaries.
1. Core concepts and terminology.
1. Invariants and safety/ordering constraints.
1. Integration points with neighboring modules.

## Type Depth

For each public type in scope, document:

1. Role in the system.
1. Semantic meaning of key fields/state.
1. Lifecycle/ownership expectations.
1. Invariants and failure/edge behavior.

## Function/Method Depth

For each function/method in scope (public and private), document:

1. Purpose in the larger flow.
1. Input assumptions and output semantics.
1. Side effects (state changes, I/O, subprocesses, ordering).

For non-trivial functions/methods, also document:

1. Failure behavior and edge cases.
1. Why the path exists when not obvious.

For dispatch/orchestration functions, document:

1. Decision order and precedence rules.
1. State transitions and reset behavior.
1. What is intentionally excluded/delegated.

## Depth Targets

- `quick` pass: document high-risk/public paths first; record remaining depth gaps.
- `full` pass: document all modules/types/functions in scope to this guide.

## Coverage Ledger Format

Use a final ledger like:

```text
Scope: <touched-files|repo-wide>
Modules documented: <n>/<n>
Types documented: <n>/<n>
Functions documented: <n>/<n>
Remaining gaps:
1. <file:line> <artifact> <missing depth>
```

## Anti-Patterns To Reject

1. Comments that only restate names/signatures.
1. Broad prose that omits constraints and edge behavior.
1. Claims without supporting evidence.
1. Generic verbs like "handles", "processes", or "manages" without contract details.

## Shallowness Signals

Treat these as likely `L0`/weak `L1` docs:

1. One-line doc with only "Returns/Builds/Handles ..." and no constraints.
1. No mention of defaults, fallbacks, or side effects for mutating functions.
1. No ordering or precedence notes for branch-heavy dispatch functions.
1. No edge/failure mention for non-trivial command/planner/conversion paths.

## Calibration Examples (Rustdoc)

### Example 1: Dispatch Hub

Too shallow:

```rust
/// Handles normal-mode shortcuts and navigation.
fn handle_normal_key(&mut self, key: KeyEvent) -> Result<(), JkError> { ... }
```

Preferred:

```rust
/// Maps a normal-mode key press to exactly one action in priority order.
///
/// Evaluation is first-match and short-circuits, so keybinding precedence is defined by branch
/// order in this function. Most branches either mutate view state in place or dispatch through
/// `execute_command_line` / `execute_with_confirmation`, which reset cursor/scroll via downstream
/// execution paths. Unrecognized keys are a no-op.
fn handle_normal_key(&mut self, key: KeyEvent) -> Result<(), JkError> { ... }
```

### Example 2: State Transition Function

Too shallow:

```rust
/// Applies a planned flow action to runtime state.
fn apply_flow_action(&mut self, action: FlowAction) -> Result<(), JkError> { ... }
```

Preferred:

```rust
/// Applies a planner action and enforces per-action state-transition invariants.
///
/// `Render` replaces visible lines, clears row-revision mapping, and resets cursor/scroll so the
/// new frame starts at a stable top position. `Execute` routes through confirmation gates for
/// dangerous commands, while `Prompt` enters prompt mode without executing side effects. `Quit`
/// only sets the termination flag; loop exit happens in the runtime driver.
fn apply_flow_action(&mut self, action: FlowAction) -> Result<(), JkError> { ... }
```

### Example 3: Fallback Resolution Logic

Too shallow:

```rust
/// Resolves selected revision from row metadata, with text parsing fallback.
fn selected_revision(&self) -> Option<String> { ... }
```

Preferred:

```rust
/// Resolves the revision nearest the cursor, preferring structured row metadata.
///
/// The lookup scans upward from the cursor and first checks `row_revision_map` entries generated
/// during output decoration. If metadata is unavailable, it falls back to parsing visible line
/// text. This preserves selection behavior across both structured and plain-output render paths.
fn selected_revision(&self) -> Option<String> { ... }
```

### Example 4: Safety Preview Planner

Too shallow:

```rust
/// Returns preview tokens for confirmation dialogs when a safe preview is available.
fn confirmation_preview_tokens(tokens: &[String]) -> Option<Vec<String>> { ... }
```

Preferred:

```rust
/// Derives a best-effort, read-only preview command for dangerous operations.
///
/// The function prefers command-specific previews (`git push --dry-run`, operation show, revset
/// logs) and falls back to recent operation-log inspection for generic tier-C commands. Returning
/// `None` means "no safe preview strategy known"; callers must still gate execution via explicit
/// confirmation.
fn confirmation_preview_tokens(tokens: &[String]) -> Option<Vec<String>> { ... }
```

## Quick Templates

### Module Template

```text
Purpose: <why this module exists>
Boundaries: <what belongs here / what does not>
Invariants: <constraints to preserve>
Interactions: <dependent modules and contracts>
```

### Type Template

```text
Role: <what this type represents>
State semantics: <meaning of key fields/state>
Lifecycle: <construction/use/disposal expectations>
Invariants: <rules that must remain true>
Failure/edges: <important caveats>
```

### Function Template

```text
Purpose: <what this does in the flow>
Inputs/Outputs: <assumptions and semantics>
Side effects: <state, I/O, subprocess, ordering>
Invariants: <constraints maintained>
Failure/Edge cases: <important behavior>
```
