# Comments

## Block JSDoc on Every Exported Declaration

`/** ... */` block form, typically a single concise sentence. Backtick-quote code-shaped terms:

```ts
/**
 * Options for the `xxx` function.
 */
type Options = {};
```

```ts
/**
 * The path to the public directory.
 */
const PATH_PUBLIC: string = Path.join(process.cwd(), "public");
```

## Per-field JSDoc on Every Field of an Exported Type

Including the default. The phrase "By default, it is `X`." recurs verbatim:

```ts
type CompleteOptions = {
    /**
     * Current working directory.
     *
     * By default, it is `process.cwd()`.
     */
    cwd: string;
};
```

## Public APIs with Example

Prefer to go with example on public APIs:

````ts
type Options = {};

type Result = {};

/**
 * Create Xxx.
 *
 * ### Example
 *
 * ```ts
 * import type { Result } from "xxx";
 *
 * import { createXxx } from "xxx";
 *
 * const xxx: Result = createXxx();
 * ```
 */
const createXxx = (options: Options): Result => {
    // ...
};

export type { Options, Result };
export { createXxx };
````

Overloaded functions get JSDoc on each overload signature, not on the implementation.

## Inline Comments

Go with inline comments when some of the logic requires some explanation to avoid them being deleted and affect the actual behavior.

## See Also

- Types: [`types.md`](./types.md)
- Functions: [`functions.md`](./functions.md)
