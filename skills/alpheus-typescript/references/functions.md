# Functions

## General

Every function takes a typed `XxxOptions` bag (see `./types.md`) for the `CompleteXxx`/`Xxx` duality used to type option bags.

## Arrow Functions Only

Prefer arrow functions unless explicitly requested.

```ts
const xxx = (options: XxxOptions): XxxResult => {};
```

## `async` / `await` Over `.then` / `.catch`

Avoid to chain `.then()` / `.catch()`. Use `async` / `await` so error paths flow through `try` / `catch` like synchronous code.

```ts
// ✗ never
const fetchThing = (): Promise<Thing> => fetch(url).then((r) => r.json());

// ✓
const fetchThing = async (): Promise<Thing> => {
    const response: Response = await fetch(url);
    return await response.json();
};
```

## Single Options-object Parameter

Use a single options-object parameter unless it have to follow specific patterns.

## Organization within a file

Helpers first, primary exported function last. All non-exported `const` arrows; the orchestrator calls them.

The functions being used by another should have higher priority in the file.

```ts
const a = (): Promise<void> => {};

const b = (): Promise<void> => {};

const c = (): Promise<void> => {
    a();
    b();
};
```

## `createXxx` factories for stateful modules

- avoid classes for application state
- return a typed object of methods

```ts
const createXxx = () => {
    return {
        isXxx: true,
        doXxx: () => {},
    };
};

type Xxx = ReturnType<typeof createXxx>;
```

## Classes for `Error` Sub Classes

You cannot extend `Error` with a factory. Every error class sets its own name:

```ts
class CustomError extends Error {
    override name: string = "CustomError";

    constructor(options: CustomErrorOptions) {
        const message: string = options.message ?? "Something went wrong";
        super(message);
    }
}
```

## Overload Idiom (Only When Public API Needs Two Call Signatures)

Two JSDoc'd public `function` overloads, then one implementation accepting the union first arg, dispatched by a type guard:

```ts
function createResponse<B extends BodyInit = BodyInit>(
    options?: CreateResponseOptions<B>,
): Response;

function createResponse<B extends BodyInit = BodyInit>(
    context?: Context,
    options?: CreateResponseOptions<B>,
): Response;

function createResponse<B extends BodyInit = BodyInit>(
    contextOrOptions?: Context | CreateResponseOptions<B>,
    options?: CreateResponseOptions<B>,
): Response {
    // ...
}
```

## See Also

- Imports & Exports: [`imports-exports.md`](./imports-exports.md)
- Types: [`types.md`](./types.md)
- Patterns: [`patterns.md`](./patterns.md)
- Comments: [`comments.md`](./comments.md)
