---
name: code-philosophy
description: "Apply this whenever writing, reviewing, or refactoring code in any language. Encodes the user's core engineering priorities: data-oriented layout, deliberate ownership and lifetimes, fixed-capacity thinking, small orthogonal modules, SOLID/DRY/KISS discipline without ceremony, and disciplined, boundary-level logging. Use for new code, code review, architecture decisions, or when the user asks for code that's 'good', 'clean', 'idiomatic', or 'production quality' in any language, not just C."
---

# Principals

1. Principals - This list governs how the skill is followed. Re-check the priorities and guidelines below against these principals before finalizing any code, review, or design decision.
2. Minimalism - No em-dashes, no emojis, no filler. Default to zero comments; add one only when removing it would cost the reader real understanding that naming or structure cannot recover.
3. Strict Validation - Never assume a language's runtime behavior, a framework's guarantee, or an existing codebase's convention. If a capacity, lifetime, ownership boundary, or existing pattern isn't stated or visible in the code, ask the user instead of guessing.

# Priorities

These are read in order. When two priorities conflict, the higher one wins, but conflicts should be rare if the design is right.

1. Correctness and clarity of ownership - who owns this memory/resource, who is allowed to mutate it, when does it die. Answer this before writing a line of logic.
2. Data layout - decide how data is stored and accessed before deciding how it is processed. The shape of the data determines the shape of the code, not the other way around.
3. Simplicity (KISS) - the simplest structure that solves the actual problem, not the one that anticipates every hypothetical future problem.
4. Single responsibility, small surface area - each function, type, and module does one job and exposes the smallest interface that does it.
5. No duplication of knowledge (DRY) - duplicated logic, not duplicated text. Two functions that happen to look similar but encode different rules are not a DRY violation. One rule copy-pasted into three places is.
6. Performance as a consequence of good structure, not a bolt-on - fast code falls out of good data layout and few allocations; don't chase micro-optimizations that fight the design.

# Data-Oriented Thinking First

Before writing any function, ask what the data actually is, how much of it there will be, and how it will be accessed in a loop. Then write the storage for that, and only then write the logic that walks it.

- Prefer struct-of-arrays over array-of-structs whenever the code will iterate over one or two fields across many instances. Group by access pattern, not by "conceptual" ownership.
- Store relationships as indices into arrays, not as pointers or object graphs. An index is stable, cheap to copy, easy to invalidate deliberately (freelist), and doesn't fight the allocator.
- Prefer fixed-capacity containers with a declared maximum over unbounded dynamic growth. Decide the capacity up front as a real design decision, not an afterthought. If growth truly must be unbounded, that's a deliberate, called-out exception, not a default.
- Batch-process. If ten thousand things need the same operation, write the loop that walks a flat array of them, don't write a virtual call per object and hope the compiler/runtime cleans it up.
- Free lists over allocate/free churn. When entries come and go, recycle slots. Don't repeatedly allocate and release growable memory for things that are actually pooled and reused.
- Use flags/bitmasks for cheap membership and capability checks over conditionals nested through an inheritance hierarchy or a chain of type checks.

# Ownership and Lifetimes

- Every allocation should have an obvious, nameable owner, and that owner's lifetime should be visible from the code, not inferred from convention or comments.
- Prefer lifetimes that are inherited from a containing context (a request, a frame, a session, a "state") over lifetimes that are managed individually and scattered. Group allocations by how long they need to live, then free/reclaim them together.
- Minimize the number of individual free/release/close calls. It is fine, and often better, to free everything at once when a context ends, rather than track and release many small pieces individually.
- If the language has manual memory management, treat "who allocates, who frees, who may not touch this after it's freed" as part of the function's contract, and make that contract obvious from naming and structure, not just a comment.
- If the language is garbage collected or reference counted, this still applies at a conceptual level: minimize the number of distinct lifetimes in flight, and prefer pooling/reuse over constant churn of short-lived objects when working with large volumes of them.

# Simplicity and Structure (KISS, SOLID, DRY)

- Solve the problem that exists. Don't add configurability, abstraction layers, or indirection for requirements that haven't been stated. Every added layer must earn its complexity cost.
- Single Responsibility in the literal sense: if describing what a function or module does requires "and," consider splitting it.
- Small, explicit interfaces. A function's signature and a type's public fields should tell you what it needs and what it promises, without reading the implementation.
- Depend on narrow, stable interfaces rather than concrete large ones (Interface Segregation / Dependency Inversion in spirit) but don't introduce an interface/abstraction for a single concrete implementation that will never have a second one. Abstraction is for real variation, not hypothetical variation.
- Open/Closed in spirit: prefer designs where adding a new case (new archetype, new event type, new state) means adding code, not editing a long chain of unrelated conditionals scattered across the codebase. A single, obvious switch/dispatch point for "add a new kind of X" is good; the same decision re-implemented in five different places is not.
- Guard clauses and early returns over deep nesting. The common/happy path should sit at the shallowest indentation.
- Prefer plain data aggregates with free functions operating on them over objects that hide state behind heavy behavior, when the problem is fundamentally "process a lot of data the same way." Reach for richer encapsulation when the problem is fundamentally about protecting an invariant, not by default.
- Code should be self-explanatory. Good names, small functions, and obvious structure should make comments unnecessary for "what is happening" in the vast majority of the code.
- Comments exist only for the "why": a non-obvious tradeoff, an invariant the type system/language can't express, a reason a seemingly-wrong-looking thing is actually correct. Never a comment that restates what the next line already says in plain sight.
- If a line or block feels like it needs a comment to explain what it does, that is a signal the code itself is not clear enough yet. Rename, extract a function, or restructure until the comment is unnecessary, rather than adding the comment.
- Default to zero comments. Add one only when removing it would genuinely cost the reader real understanding that better naming or structure cannot recover.

# Modularity

- One clear responsibility per module/subsystem, with dependencies between modules that are explicit and visible (explicit imports/includes/build targets), never ambient or implicit.
- A module's public surface should be small and deliberate. Internal helpers stay internal.
- Mirror logical separation with physical separation: if two things are conceptually distinct subsystems, they should live in distinct files/folders/build units, not tangled into one "misc" or "utils" catch-all.

# Failure Handling

- Fail loudly and immediately on programmer error (invalid arguments, broken invariants) during development; fail soft and recoverably on expected runtime conditions (capacity reached, not found, disconnected).
- Validate preconditions at the top of a function, before any work happens.
- Prefer returning a clear failure signal (bool/error/optional/result) over silently doing nothing or silently truncating behavior.

# Logging

- Log at decision points and boundaries - where a failure is handled, where control crosses into/out of a module, where an external system is called - not as a narration of internal steps within a function.
- Every log line carries the identifiers needed to correlate it back to the causing state (an id, an index, a count, a slot), not vague prose like "something went wrong."
- Log a failure once, at the point it's decided/handled, not again at every layer it bubbles through - one caught-and-logged error, not the same failure re-logged at three call sites.
- Match log level to audience and frequency: error/warn for things a human should act on or investigate, info for coarse-grained lifecycle events, debug/trace for anything that only matters mid-development or during active troubleshooting - and gate debug/trace so it doesn't run by default in hot loops.
- A log call never substitutes for the failure-handling contract - logging a problem doesn't replace returning the clear failure signal (bool/error/optional/result) the caller needs. Do both, not one instead of the other.
- Structured over stringly-typed where the language/logging system supports it (key-value fields, not everything baked into one formatted sentence), so logs stay greppable/queryable at scale.
- No log statements left over from debugging a specific incident - once the incident's understood, remove throwaway prints/logs or promote them to a real, leveled log line with a permanent reason to exist.

# Naming and Micro-Style

- `subject_action` naming for functions (`stack_push`, not `push_to_stack` or a bare `push`), so related operations sort and scan together.
- A "thing" and a "thing manager/registry" is a good pattern when many instances of something need centralized registration, bookkeeping, or lookup, separate from the lightweight per-instance record itself.
- Mark trivial, hot accessors and predicates for inlining when the language supports the hint. Don't let a one-line abstraction cost a real function call in a hot loop.
- Consistent, generous vertical whitespace between logical sections (includes/imports, types, functions) is preferred over cramming everything together. Readability of structure at a glance matters.
- Prefer explicit, narrow imports over broad "import everything" headers/modules, so a file's real dependencies are visible from its top.

# What Good Output Looks Like

When writing new code under this skill, the result should be recognizable by:
- Data structures defined before the logic that uses them, with a clear note on who owns them and how long they live.
- Preallocated or pooled storage for anything that repeats at scale, instead of allocate-per-item.
- A short list of failure/edge cases handled with early returns, not buried in nested conditionals.
- Functions that do one thing, named so the one thing is obvious, with no dead configurability for requirements that don't exist yet.
- No duplicated business rules. Shared logic factored into one place; coincidentally-similar-looking but independently-varying logic left alone.

# Example

Prompt: "Write something that manages a pool of enemies in a game, where enemies get added and removed a lot."

Naive version (what NOT to default to): a growable list of enemy objects, each individually heap-allocated, removed via a linear search-and-erase, with per-enemy virtual dispatch for update/render.

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

No comments were needed. `EnemyPool` names its own shape, `free_list`/`free_count` name the recycling scheme, `MAX_ENEMIES` names the capacity decision, and `enemy_is_alive` turns a bare health check into a named concept the update loop can just call. The one thing worth a real comment would be an actual "why" - e.g. if `ENEMY_FALL_SPEED` were tuned to match a specific animation frame count for a design reason invisible from the code - and even then, only at the constant's definition, not scattered through the functions that use it.
