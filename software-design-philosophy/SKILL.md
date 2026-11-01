---
name: software-design-philosophy
description: Evaluate software designs to reduce complexity and make code easier to change. Use for code reviews, architecture and API design, module decomposition, refactoring, and error handling. Apply to questions about coupling, boundaries, interface depth, naming, duplicated knowledge, premature abstraction, or when to commit to a design choice.
---

# Software Design Philosophy

Use this skill to diagnose design problems and explain their consequences. It combines
Ousterhout's complexity lens with changeability and commitment-timing guidance from
*The Pragmatic Programmer*, Lean software development, Real Options, and YAGNI.

## How to apply this

1. **Diagnose before prescribing.** Identify the complexity symptom and the dependency or
   obscurity causing it. Use the red flags in Section 14 to name the problem precisely.
2. **Explain consequences and trade-offs.** Connect advice to the code, rather than citing
   rules. Compare designs against a concrete, plausible change (Easier to Change, or ETC),
   including today's interface and maintenance costs. A local special case can be
   reasonable under a real deadline; hypothetical flexibility alone does not justify
   abstraction.
3. **Check commitment timing.** For consequential choices, assess uncertainty, reversal
   cost, and what further work could teach you. Make reversible progress; defer costly
   commitments when useful evidence is forthcoming and alternatives remain viable. State
   the learning action and commitment trigger, allowing time for implementation and
   validation. Decide sooner when delay blocks work, increases risk, or adds little
   information. Clear boundaries can preserve options; extra abstraction needs a current
   benefit.
4. **Load references selectively.** Use the table below for examples or deeper reasoning.
   Read only the relevant sections; do not recite the references by default. This skill
   complements repository review tooling rather than replacing its mechanical checks.

| Question or symptom | Reference |
| --- | --- |
| Passing tests versus design quality | [Examples](references/examples.md): text-editor class study |
| Storage-shaped APIs, callers splitting/joining, UI types in data classes | [Examples](references/examples.md): text-editor Text class API |
| Wide interface with little functionality | [Examples](references/examples.md): Unix I/O vs Java streams |
| Error handling imposed on callers | [Examples](references/examples.md): unlink vs Windows delete; Java checked exceptions |
| Internal choices exposed as caller configuration | [Examples](references/examples.md): hash map resizing |
| DRY, independent concerns, competing abstractions | [Changeability](references/changeability.md) |
| Uncertain architecture, speculative flexibility, costly migrations | [Decision timing](references/decision-timing.md) |
| Prototype versus tracer bullet | [Learning techniques](references/decision-timing.md#choosing-a-learning-technique) |

## 1. Complexity

Complexity makes a system hard to understand or modify, regardless of size. It accumulates
through small decisions, so examine seemingly minor interface, naming, and error-path costs.

**Three symptoms:**

- **Change amplification:** a simple conceptual change requires edits in many places.
- **Cognitive load:** a developer must hold too much information to make a safe change.
  Fewer lines are not necessarily simpler; explicit code can reduce inference.
- **Unknown unknowns:** it is unclear what needs to change or which information matters,
  making omissions difficult to detect.

**Two root causes:**

- **Dependencies:** code cannot be understood or changed without understanding other code.
- **Obscurity:** important information is hidden by vague names, implicit conventions,
  missing context, or unstated constraints.

## 2. Strategic vs. tactical programming

**Tactical programming** prioritizes getting a feature working while leaving design debt
for later. Special cases and unexplained TODOs can compound into expensive future changes.

**Strategic programming** invests continually in design alongside working behavior.
Ousterhout suggests roughly 10–20% of development time for improvements: seek opportunities
with each change rather than postponing them to a large refactor. Assess progress through
abstraction quality as well as feature completion.

Deliberately deferring a costly commitment can be strategic when it has a learning action
and a concrete trigger for deciding. Distinguish that from neglecting design.

## 3. Deep modules

A module is **deep** when a simple interface hides substantial functionality. It is
**shallow** when its interface is nearly as complex as its implementation.

Unix file I/O illustrates depth: `open`, `read`, `write`, `lseek`, and `close` hide a
filesystem. Java streams illustrate interface cost: reading a file may require composing
`FileInputStream`, `BufferedInputStream`, and `ObjectInputStream`.

Make common operations easy for callers, even if implementation becomes harder. Rare
features should not complicate the common path. Optimize interface simplicity, not just
implementation size.

## 4. Information hiding and orthogonality

Each module should hide a design decision: a data structure, algorithm, mechanism, or
policy. **Information leakage** occurs when that decision appears across modules, so
changing it requires coordinated edits.

**Temporal decomposition** often causes leakage: modules follow execution order instead
of grouping the information they need to hide. Sequential steps may share a decision
that belongs in one place.

Merge modules that share the decision, or concentrate it behind a deep module they can
both use. Keep unrelated concerns **orthogonal**: a display change should not require
changing persistence policy. Trace actual dependencies and shared state; splitting a
cohesive module into shallow wrappers can make change harder.

## 5. General-purpose vs. special-purpose modules

Ask: *What is the most general interface that satisfies current needs?* General-purpose
interfaces can be deeper, serving more use cases with fewer, more powerful operations.
This concerns interface shape; implement only what is needed today.

A general interface that simplifies current callers earns its place. Extension points,
configuration, and frameworks for hypothetical requirements need separate justification.
Keep general-purpose mechanisms separate from special-purpose policy.

For example, a text interface offering `insert`, `delete`, and `changePosition` can hide
more complexity than `getLine`/`putLine` or UI-specific `backspace(Cursor)` methods.

## 6. Different layers, different abstractions

Each layer should contribute a different abstraction. Similar adjacent layers suggest
misplaced responsibilities:

- **Pass-through method:** forwards arguments with a near-identical signature, adding
  little abstraction.
- **Pass-through variable:** travels through layers that never use it. Consider a context
  object, dependency injection, or different boundaries to avoid threading it everywhere.

## 7. Pull complexity downward

When complexity cannot be eliminated, prefer resolving it inside the module over imposing
it on every caller. The module usually has fewer maintainers than call sites.

Watch for hard internal choices exposed as configuration and exceptions that callers
must handle even though the module could resolve them. Both can save local effort while
multiplying complexity across the system.

## 8. Define errors out of existence

Reduce unnecessary error paths through interface semantics and local handling:

- **Redefine semantics:** a `delete(key)` operation can guarantee absence whether or not
  the key existed. A substring operation may clip indices if that matches its contract.
- **Exception masking:** resolve a condition locally so callers need no error path.
- **Exception aggregation:** handle related exceptions together instead of scattering
  equivalent handlers across callers.

Preserve errors that matter. A dropped network packet needs appropriate handling;
simplifying an interface does not justify hiding failures or changing required behavior.

## 9. Design it twice

For consequential design decisions, sketch two distinct approaches before choosing.
Compare interface simplicity, generality, performance, and implementation effort; an
alternative can expose weaknesses in the first design.

Comparison need not force immediate commitment. Use a focused experiment or reversible
implementation when important uncertainty remains, accounting for the cost of preserving
multiple options.

## 10. Comments

Comments capture intent, constraints, edge cases, and side effects that code cannot
express. Write interface comments before implementation to test whether the abstraction
is clear. Callers should understand the contract without reading the implementation.

- **Interface comments:** behavior and purpose without unnecessary implementation detail.
- **Implementation comments:** internal reasoning and mechanisms.
- **Cross-module comments:** decisions or dependencies invisible within a single module.

A comment that merely translates code into English adds no information and risks becoming
stale. Difficulty explaining a module can indicate an unsettled or overly complex design.

## 11. Naming

Names should be precise, informative, and consistent across the codebase.

- **Vague name:** `data`, `result`, `tmp`, or `info` conveys little about its role.
- **Hard to name:** after reasonable effort, difficulty finding a clear name may indicate
  an ill-defined entity. Reconsider its responsibility before inventing terminology.

## 12. Consistency

Use consistent names, styles, interface patterns, and invariants so knowledge transfers
across the codebase. A marginal improvement rarely justifies introducing a competing
convention; weigh the benefit against migration and ongoing inconsistency costs.

## 13. Obvious code

A reader should understand behavior and intent without excessive inference. Use clear
names, structure, and comments, and examine implicit control flow such as surprising
callback chains. Judge the reader's understanding, not syntactic neatness.

## 14. Red flags quick reference

| Signal | Meaning |
| --- | --- |
| Shallow Module | Interface is nearly as complex as its implementation |
| Information Leakage | One design decision spans modules that must change together |
| Temporal Decomposition | Modules follow execution order instead of information hiding |
| Overexposure | Common operations require knowledge of rare features |
| Pass-Through Method | Forwards calls without adding a useful abstraction |
| Pass-Through Variable | Threads through layers that never use it |
| Duplicated Knowledge (DRY) | One rule has representations that must stay synchronized; similar code alone is insufficient evidence |
| Special-General Mixture | General mechanisms and special-purpose policy are tangled |
| Conjoined Methods | Understanding one method requires reading another |
| Comment Repeats Code | Restates code without adding information |
| Impl. Contaminates Interface | Contract exposes internal details callers do not need |
| Vague Name | Name gives little information about its role |
| Hard to Pick a Name | Entity may lack a clear responsibility |
| Hard to Describe | Explanation difficulty may indicate excessive complexity |
| Non-Obvious Code | Reader cannot readily infer behavior or intent |
