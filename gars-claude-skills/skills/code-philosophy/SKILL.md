---
name: code-philosophy
description: "Apply this whenever writing, reviewing, or refactoring code in any language. Encodes the user's core engineering priorities: data-oriented layout, deliberate ownership and lifetimes, fixed-capacity thinking, small orthogonal modules, SOLID/DRY/KISS discipline without ceremony, and disciplined, boundary-level logging. Use for new code, code review, architecture decisions, or when the user asks for code that's 'good', 'clean', 'idiomatic', or 'production quality' in any language, not just C."
---

# Rules

1. Default to zero comments.
2. Never assume a runtime behavior, framework guarantee, or codebase convention. If a capacity, lifetime, ownership boundary, or existing pattern isn't stated or visible in the code, ask.

# Priorities

Read in order; higher wins on conflict, but conflicts are rare if the design is right.

1. Ownership clarity - who owns this memory/resource, who may mutate it, when it dies. Answer before writing logic.
2. Data layout - decide storage and access before processing. Data shape determines code shape, not the reverse.
3. Simplicity (KISS) - simplest structure solving the actual problem, not every hypothetical one.
4. Single responsibility, small surface area - one job per function/type/module, smallest interface that does it.
5. DRY on knowledge, not text - one rule copy-pasted into three places is a violation; two similar-looking functions encoding different rules are not.
6. Performance as a consequence of structure - speed falls out of good layout and few allocations; don't bolt on micro-optimizations that fight the design.

# Data-Oriented Thinking First

Before any function: what is the data, how much of it, how is it accessed in a loop. Write the storage, then the logic that walks it.

- Struct-of-arrays over array-of-structs when iterating one or two fields across many instances. Group by access pattern, not conceptual ownership.
- Store relationships as indices, not pointers or object graphs. Indices are stable, cheap to copy, deliberately invalidatable (freelist), and don't fight the allocator.
- Fixed-capacity containers with a declared maximum over unbounded growth. Decide capacity up front as a real design decision. Unbounded growth is a called-out exception, never a default.
- Batch-process. Ten thousand things needing the same operation get one loop over a flat array, not a virtual call per object.
- Free lists over allocate/free churn. Recycle slots for things that are pooled and reused.
- Flags/bitmasks for membership and capability checks, over conditionals nested through an inheritance hierarchy or chain of type checks.

# Ownership and Lifetimes

- Every allocation has an obvious, nameable owner whose lifetime is visible in the code, not inferred from convention or comments.
- Prefer lifetimes inherited from a containing context (request, frame, session, state) over individually managed, scattered ones. Group allocations by how long they live, reclaim them together.
- Minimize individual free/release/close calls. Freeing everything at once when a context ends is fine and often better than tracking many small pieces.
- Under manual memory management, "who allocates, who frees, who may not touch this after free" is part of the function contract, made obvious by naming and structure, not a comment.
- Under GC or refcounting the same applies conceptually: minimize distinct lifetimes in flight, prefer pooling/reuse over churn of short-lived objects at volume.

# Simplicity and Structure (KISS, SOLID, DRY)

- Solve the problem that exists. No configurability, abstraction, or indirection for unstated requirements. Every layer earns its complexity cost.
- Single Responsibility literally: if describing a function or module needs "and," consider splitting it.
- Small, explicit interfaces. A signature and public fields tell you what is needed and promised without reading the implementation.
- Depend on narrow, stable interfaces over concrete large ones (Interface Segregation / Dependency Inversion in spirit), but never introduce an abstraction for a single implementation that will never have a second. Abstraction is for real variation, not hypothetical.
- Open/Closed in spirit: adding a new case (archetype, event type, state) should mean adding code at one obvious switch/dispatch point, not editing a chain of unrelated conditionals scattered across the codebase.
- Guard clauses and early returns over deep nesting. The happy path sits at the shallowest indentation.
- Plain data aggregates with free functions over objects hiding state behind heavy behavior, when the problem is "process a lot of data the same way." Reach for richer encapsulation only when protecting an invariant.
- Code is self-explanatory: good names, small functions, obvious structure make "what is happening" comments unnecessary almost everywhere.
- Comments exist only for "why": a non-obvious tradeoff, an invariant the language can't express, a reason something seemingly wrong is correct. Never a restatement of the next line.
- A block that feels like it needs a comment to explain what it does is a signal the code isn't clear enough. Rename, extract, or restructure instead of commenting.
- Default to zero comments. Add one only when removing it would cost the reader real understanding that naming or structure cannot recover.

# Modularity

- One clear responsibility per module/subsystem, with explicit, visible dependencies (explicit imports/includes/build targets), never ambient.
- Small, deliberate public surface. Internal helpers stay internal.
- Mirror logical separation with physical separation: conceptually distinct subsystems live in distinct files/folders/build units, not a "misc" or "utils" catch-all.

# Failure Handling

- Fail loudly and immediately on programmer error (invalid arguments, broken invariants) during development; fail soft and recoverably on expected runtime conditions (capacity reached, not found, disconnected).
- Validate preconditions at the top of a function, before any work.
- Return a clear failure signal (bool/error/optional/result) over silently doing nothing or truncating behavior.

# Logging

- Log at decision points and boundaries - where a failure is handled, where control crosses a module edge, where an external system is called - not as narration of internal steps.
- Every line carries the identifiers needed to correlate it to the causing state (id, index, count, slot), not vague prose.
- Log a failure once, where it is decided/handled, not again at every layer it bubbles through.
- Match level to audience and frequency: error/warn for what a human must act on, info for coarse lifecycle events, debug/trace for mid-development or active troubleshooting, gated so it doesn't run by default in hot loops.
- Logging never substitutes for the failure-handling contract. Log and return the failure signal, not one instead of the other.
- Structured over stringly-typed where supported (key-value fields, not one formatted sentence), so logs stay greppable at scale.
- No leftover debugging logs. Once an incident is understood, remove throwaway prints or promote them to real, leveled lines with a permanent reason to exist.

# Naming and Micro-Style

- `subject_action` naming (`stack_push`, not `push_to_stack` or bare `push`), so related operations sort and scan together.
- A "thing" plus a "thing manager/registry" is a good pattern when many instances need centralized registration, bookkeeping, or lookup separate from the lightweight per-instance record.
- Mark trivial hot accessors and predicates for inlining where the language supports the hint. A one-line abstraction shouldn't cost a real call in a hot loop.
- Generous, consistent vertical whitespace between logical sections (imports, types, functions) over cramming.
- Explicit, narrow imports over broad "import everything," so a file's real dependencies are visible at its top.

# What Good Output Looks Like

- Data structures defined before the logic using them, with a clear note on owner and lifetime.
- Preallocated or pooled storage for anything repeating at scale, not allocate-per-item.
- A short list of failure/edge cases handled with early returns, not buried in nested conditionals.
- Functions doing one obvious thing, named so, with no dead configurability.
- No duplicated business rules. Shared logic in one place; coincidentally-similar but independently-varying logic left alone.

# Example

Prompt: "Write something that manages a pool of enemies in a game, where enemies get added and removed a lot."

Naive version (what NOT to default to): a growable list of individually heap-allocated enemy objects, removed via linear search-and-erase, with per-enemy virtual dispatch for update/render.

Applying this skill instead:

```
struct EnemyPool
{
    Vec2   position[MAX_ENEMIES];
    float  health[MAX_ENEMIES];
    u32    free_list[MAX_ENEMIES];
    u32    free_count;
}

u32 enemy_spawn(EnemyPool *pool, Vec2 spawn_position)
{
    if (pool->free_count == 0) return INVALID_ENEMY;

    u32 slot = pool->free_list[--pool->free_count];
    pool->position[slot] = spawn_position;
    pool->health[slot] = ENEMY_DEFAULT_HEALTH;
    return slot;
}

void enemy_kill(EnemyPool *pool, u32 slot)
{
    assert(slot < MAX_ENEMIES);

    pool->health[slot] = 0.f;
    pool->free_list[pool->free_count++] = slot;
}

bool enemy_is_alive(EnemyPool *pool, u32 slot)
{
    return pool->health[slot] > 0.f;
}

void enemy_update_all(EnemyPool *pool, float dt)
{
    for (u32 i = 0; i < MAX_ENEMIES; i++)
    {
        if (!enemy_is_alive(pool, i)) continue;
        pool->position[i].y += ENEMY_FALL_SPEED * dt;
    }
}
```

No comments needed: `EnemyPool` names its shape, `free_list`/`free_count` name the recycling scheme, `MAX_ENEMIES` names the capacity decision, `enemy_is_alive` turns a bare health check into a named concept. The one thing worth a real comment would be an actual "why" - e.g. `ENEMY_FALL_SPEED` tuned to a specific animation frame count for a design reason invisible from the code - and then only at the constant's definition, not in the functions using it.
