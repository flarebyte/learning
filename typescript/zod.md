## Zod 4 — The Next Evolution in Type-Safe Validation

Zod is a TypeScript-first schema declaration and validation library that brings runtime safety into your code without losing the power of static types. With **Zod 4**, you get:

- **Trusted, production-ready stability** — the new version is now officially stable.
- **Full backward compatibility + smooth migration path** — updates aim to enhance without breaking.
- **Stronger tooling** — better error handling, metadata support, JSON Schema integration, and refined API ergonomics.
- **Lean and fast** — still zero external dependencies and optimized for both Node.js and browser usage.
- **Deep TypeScript synergy** — define your schemas and infer types effortlessly, keeping runtime validation and compile-time safety in perfect harmony.

## Import & Basics

```ts
import { z } from "zod";

const User = z.object({
  id: z.string().uuid(),
  name: z.string().min(1),
  age: z.number().int().nonnegative().optional(),
});

type User = z.infer<typeof User>; // output type
type UserIn = z.input<typeof User>; // pre-parse (input) type
type UserOut = z.output<typeof User>; // post-parse (after transforms)

User.parse(data); // throws on error
User.safeParse(data); // { success: boolean; data|error }
await User.parseAsync(x); // for async refinements
```

---

## Primitives

```ts
z.string()
  .min(n) .max(n) .length(n)
  .email() .url() .uuid() .cuid() .cuid2() .ulid()
  .regex(/.../, "msg") .includes("x") .startsWith("x") .endsWith("x")
  .datetime({ precision?: 0|1|...; offset?: boolean }) // ISO 8601
  // string transforms:
  .trim() .toLowerCase() .toUpperCase()

z.number()
  .int() .finite() .safe()
  .nonnegative() .nonpositive() .positive() .negative()
  .gte(n) .gt(n) .lte(n) .lt(n) .multipleOf(n)

z.boolean();
z.bigint();                 // .gt/.gte/.lt/.lte available
z.symbol();
z.date()                    // .min(date) .max(date)
z.undefined(); z.null(); z.void(); z.nan();

z.literal("A");             // literal value
z.enum(["A","B","C"]);      // string enum
z.nativeEnum(MyTsEnum);     // TS enum

z.any(); z.unknown(); z.never();
```

---

## Collections & Structs

```ts
z.object({ a: z.string() })
  .passthrough()     // allow unknown keys
  .strict()          // forbid unknown keys (default)
  .strip()           // drop unknown keys
  .extend({ b: z.number() })
  .merge(otherObj)
  .pick({ a: true }) .omit({ a: true })
  .partial()         // all keys optional
  .deepPartial()
  .required()        // all keys required
  .catchall(z.any()) // schema for unknown keys
  .keyof()           // -> z.ZodEnum of keys

z.array(z.string())
  .min(n) .max(n) .length(n)
  .nonempty()        // Non-empty array
  .element;          // (type helper)

z.tuple([z.string(), z.number()])
  .rest(z.boolean()); // variadic tail

z.record(z.string(), z.number()); // { [key: string]: number }
z.map(z.string(), z.number());    // Map<string, number>
z.set(z.string());                // Set<string>

z.union([A, B, C]);
z.discriminatedUnion("tag", [
  z.object({ tag: z.literal("a"), ... }),
  z.object({ tag: z.literal("b"), ... }),
]);
z.intersection(A, B);
z.lazy(() => Node);               // recursive types
```

---

## Functions & Promises

```ts
z.function()
  .args(z.string(), z.number().optional()) // or .args(z.tuple([...]))
  .returns(z.boolean());

z.promise(z.string()); // Promise<string>
```

---

## Coercion (accept loose input -> typed output)

```ts
z.coerce.string(); // 123 -> "123"
z.coerce.number(); // "42" -> 42 (NaN fails)
z.coerce.boolean(); // "true"/1 -> true
z.coerce.bigint();
z.coerce.date(); // "2025-10-05" -> Date
```

---

## Optional / Nullable / Defaults / Fallbacks

```ts
z.string().optional(); // T | undefined
z.string().nullable(); // T | null
z.string().nullish(); // T | null | undefined
z.string().default("x"); // supplies missing/undefined
z.string().catch("fallback"); // on parse error, return fallback
```

---

## Refinements, Transforms, Effects

```ts
// Boolean guard + message
z.string().refine((v) => v.includes("@"), {
  message: "Must contain @",
  path: ["email"],
});

// Multi-issue validation
z.string().superRefine((v, ctx) => {
  if (!v.includes("@"))
    ctx.addIssue({ code: "custom", message: "Email missing @" });
});

// Transform value
z.string().transform((s) => s.trim());

// Pipe (validate -> transform -> validate again)
z.string().trim().pipe(z.string().min(3)); // enforces result length >= 3

// Preprocess (run before validation)
z.preprocess((x) => (typeof x === "string" ? x.trim() : x), z.string().min(1));
```

---

## Instances & Custom

```ts
z.instanceof(Date);
z.custom<Brand>((val) => isBrand(val), { message: "Not Brand" });
```

---

## Branding & Descriptions

```ts
const UserId = z.string().uuid().brand<"UserId">();
type UserId = z.infer<typeof UserId> & { readonly __brand: "UserId" }; // branded nominal-ish

const S = z.string().describe("User-friendly description"); // surfaces in errors/metadata
```

---

## Error Handling

```ts
try {
  Schema.parse(x);
} catch (e) {
  if (e instanceof z.ZodError) {
    e.issues; // flat list
    e.format(); // tree shaped by object paths
    e.errors; // legacy alias
  }
}
```

---

## JSON & “Unknown” Inputs (patterns)

```ts
// Strict JSON value type
const Json: z.ZodType<JsonValue> = z.lazy(() =>
  z.union([
    z.string(),
    z.number(),
    z.boolean(),
    z.null(),
    z.array(Json),
    z.record(Json),
  ])
);

// Parse then JSON.parse
const JsonString = z.string().transform((s, ctx) => {
  try {
    return JSON.parse(s);
  } catch {
    ctx.addIssue({ code: "custom", message: "Invalid JSON" });
    return z.NEVER;
  }
});
```

---

## Practical Patterns

**Disallow empty string but allow undefined**

```ts
const NonEmptyOpt = z
  .string()
  .min(1)
  .optional()
  .transform((v) => v ?? undefined);
```

**ISO date string → Date (validated)**

```ts
const IsoDate = z
  .string()
  .datetime()
  .transform((s) => new Date(s));
```

**Form input coercion**

```ts
const Form = z.object({
  age: z.coerce.number().int().gte(0),
  subscribed: z.coerce.boolean(),
});
```

**Narrowing a union by predicate**

```ts
const A = z.object({ kind: z.literal("a"), x: z.number() });
const B = z.object({ kind: z.literal("b"), y: z.string() });
const AB = z.union([A, B]);
```

---

## Type Utilities (handy in TS)

```ts
type T = z.infer<typeof Schema>;
type Input = z.input<typeof Schema>;
type Output = z.output<typeof Schema>;
```

---

### Mini “Grammar” Index (at a glance)

- Constructors: `z.string z.number z.boolean z.bigint z.symbol z.date z.undefined z.null z.void z.nan z.literal z.enum z.nativeEnum z.object z.array z.tuple z.union z.discriminatedUnion z.intersection z.record z.map z.set z.function z.promise z.instanceof z.lazy z.any z.unknown z.never z.custom`
- Modifiers: `.optional .nullable .nullish .default .catch .brand .describe`
- String checks: `.min .max .length .email .url .uuid .cuid .cuid2 .ulid .regex .includes .startsWith .endsWith .datetime`
- String transforms: `.trim .toLowerCase .toUpperCase`
- Number checks: `.int .finite .safe .gt .gte .lt .lte .positive .negative .nonnegative .nonpositive .multipleOf`
- Array checks: `.min .max .length .nonempty`
- Object helpers: `.partial .deepPartial .required .pick .omit .extend .merge .passthrough .strict .strip .catchall .keyof`
- Flow: `.refine .superRefine .transform .pipe` and `z.preprocess`
- Coercion: `z.coerce.string|number|boolean|bigint|date`
- Parse: `.parse .safeParse .parseAsync .safeParseAsync`

Here’s a concise, copy-paste friendly section you can slot under your cheatsheet.

# Migrating from Zod v2 to v4

## What typically changes

1. Object unknown-key policy is explicit
   In older code you might see patterns like a non-strict object plus ad-hoc key filtering. In v4, pick one of the explicit modes per schema:

   ```ts
   // strict: only declared keys allowed; unknown keys cause an error
   z.object({...}).strict();

   // strip: unknown keys are removed during parsing
   z.object({...}).strip();

   // passthrough: unknown keys are preserved
   z.object({...}).passthrough();
   ```

   If you used a single global pattern before, standardize per schema now.

2. Input vs output types are first-class
   Prefer `z.input<typeof S>` and `z.output<typeof S>` when a schema transforms or coerces:

   ```ts
   const S = z.object({ age: z.coerce.number().int() });
   type AgeIn = z.input<typeof S>; // string | number
   type AgeOut = z.output<typeof S>; // number
   ```

3. Effects are more composable
   Use `.pipe` to sequence validation and transformation cleanly, and prefer `ctx.addIssue(...)` over throwing inside transforms:

   ```ts
   const Email = z
     .string()
     .trim()
     .pipe(z.string().min(3))
     .refine((v) => v.includes("@"), { message: "Must include @" });

   const ParsedJson = z.string().transform((s, ctx) => {
     try {
       return JSON.parse(s);
     } catch {
       ctx.addIssue({ code: "custom", message: "Invalid JSON" });
       return z.NEVER;
     }
   });
   ```

4. Coercion is explicit
   Replace implicit preprocessing with `z.coerce.*` where you accept loose input:

   ```ts
   const Form = z.object({
     subscribed: z.coerce.boolean(),
     count: z.coerce.number().int().gte(0),
   });
   ```

5. Discriminated unions scale better
   If you previously refined a union manually, switch to `z.discriminatedUnion` with a literal tag key for faster, clearer parsing.

6. Error handling surfaces richer metadata
   `ZodError` keeps `issues`, `format()`, and paths stable. Centralize error normalization:

   ```ts
   function toFieldErrors(e: unknown) {
     if (e instanceof z.ZodError) return e.format();
     return { _errors: ["Unknown validation error"] };
   }
   ```

## Tooling upgrades that help

- TypeScript strictness: enable `strict`, `noUncheckedIndexedAccess`, and keep `exactOptionalPropertyTypes` in mind for object schemas.
- Explicit “input vs output” typing: annotate DTO boundaries with `z.input` on inbound and `z.output` on outbound to prevent accidental reliance on pre-transform shapes.
- Linting: add rules that forbid `any` at API edges and encourage centralized schema exports. Consider a custom lint rule or code review checklist to require schemas for all external inputs.
- Tests: add schema-based property tests or table tests for known bad cases, and snapshot `ZodError.format()` for stable, reviewable error shapes.
- Runtime guards: wrap `safeParse` in small helpers so calling code never forgets to branch on success.

  ```ts
  const result = Schema.safeParse(data);
  if (!result.success) return { ok: false, error: result.error };
  return { ok: true, value: result.data };
  ```

## Paste-ready upgrade commands

```bash
# see current
npm ls zod

# upgrade to latest v4
npm i -D @types/node # if needed for TS baselines
npm i zod@^4.1.11

# optional: codemod for v3→v4
npx zod-v3-to-v4
```

## Short migration checklist

- Replace any ad-hoc unknown-key handling with `.strict()`, `.strip()`, or `.passthrough()` on every `z.object(...)`.
- Wherever a schema transforms or coerces, update types at call sites to use `z.input<typeof S>` and `z.output<typeof S>`.
- Convert manual refine-then-transform chains to `.pipe(...)` where appropriate; use `ctx.addIssue` and `z.NEVER` for transform failures.
- Swap legacy preprocess patterns that emulate parsing to numbers, booleans, or dates for `z.coerce.number|boolean|date`.
- For tagged unions validated by custom predicates, move to `z.discriminatedUnion("tag", [...])`.
- Standardize error handling: catch `ZodError`, use `format()` for UI, and normalize to a consistent return shape from validators.
- Add tests that cover both happy and unhappy paths for each public schema, including unknown keys, nullish inputs, and boundary values.
- Run a project-wide search for `.parse(`; where exceptions would be undesirable, replace with `safeParse` and handle failures explicitly.
- Re-generate or re-review any downstream types that were inferred indirectly from schemas to ensure they match post-transform outputs.
- Audit API boundaries, form handlers, and database adapters to confirm all external inputs are validated by a schema before use.

## What changed from v3 → v4 for `ZodIssue`

- **Issue type names were consolidated/renamed** and are now exposed under Zod’s _core_ types (the `$`-prefixed ones). The practical effect: fewer distinct issue kinds, plus two new ones for keyed/element containers.
  **v3 → v4 mapping (high-level):**

  - `ZodInvalidTypeIssue` → `$ZodIssueInvalidType`
  - `ZodTooBigIssue` → `$ZodIssueTooBig`
  - `ZodTooSmallIssue` → `$ZodIssueTooSmall`
  - `ZodInvalidStringIssue` → `$ZodIssueInvalidStringFormat`
  - `ZodNotMultipleOfIssue` → `$ZodIssueNotMultipleOf`
  - `ZodUnrecognizedKeysIssue` → `$ZodIssueUnrecognizedKeys`
  - `ZodInvalidUnionIssue` → `$ZodIssueInvalidUnion`
  - `ZodCustomIssue` → `$ZodIssueCustom`
  - **Merged into one:** `ZodInvalidEnumValueIssue` and `ZodInvalidLiteralIssue` → **`$ZodIssueInvalidValue`**
  - **New:** `$ZodIssueInvalidKey` (for `z.record`/`z.map`) and `$ZodIssueInvalidElement` (for `z.map`/`z.set`)
  - Some former issues now **throw regular `Error`s** at _schema creation_ (e.g., invalid union discriminator), or map to `invalid_type` (e.g., invalid date). ([Zod][1])

- **Base issue interface updated** (still structurally similar). In v4, issues share a base like:

  ```ts
  interface $ZodIssueBase {
    readonly code?: string;
    readonly input?: unknown;
    readonly path: PropertyKey[];
    readonly message: string;
  }
  ```

## Minimal migration tips (code)

** Update your type imports / guards**
If you did explicit type narrowing against v3 issue interfaces, prefer _structural_ checks in v4:

```ts
import { z } from "zod";

try {
  schema.parse(input);
} catch (e) {
  if (e instanceof z.ZodError) {
    for (const issue of e.issues) {
      // v4-safe structural guards:
      if (issue.code === "too_small") {
        // issue has .minimum, .inclusive, .type ("array" | "string" | "set" | "number")
      } else if (issue.code === "invalid_value") {
        // replaces enum/literal invalids
      } else if (
        issue.code === "invalid_key" ||
        issue.code === "invalid_element"
      ) {
        // new in v4
      }
    }
  }
}
```

** If you had custom formatting based on v3 issue kinds**, remap them like so:

- v3 `invalid_enum_value` **or** `invalid_literal` → v4 `invalid_value`
- v3 `invalid_date` → v4 `invalid_type` (date)
  …and account for `invalid_key` / `invalid_element` where you format container problems.

## **to-do list for updating `ZodIssue` handling in an AI agent (v3 → v4):**

- [ ] Switch from `error.errors` to `error.issues` when inspecting parse failures.
- [ ] Match on **`issue.code`** (e.g. `"invalid_value"`, `"too_small"`, `"invalid_key"`) instead of v3 issue type names.
- [ ] Remap old codes:

  - `invalid_enum_value` / `invalid_literal` → `invalid_value`
  - add support for new `invalid_key` / `invalid_element`

- [ ] Use **schema-level `error`** instead of `message`, `errorMap`, `invalid_type_error`, `required_error`.
- [ ] Expect `.default()` / `.catch()` values to apply even when optional fields are absent.
- [ ] Use **`safeParse`** for agent loops; branch on success/failure.
- [ ] Leverage helpers for formatting:

  - `flattenError` for form/tool args
  - `treeifyError` for structured repair
  - `prettifyError` for human/LLM messaging
