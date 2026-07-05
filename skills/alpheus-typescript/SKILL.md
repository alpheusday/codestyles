---
name: alpheus-typescript
description: Use when writing or editing JavaScript or TypeScript code to enforce Alpheus coding style.
---

# Alpheus Coding Style (TypeScript)

## Overview

- prefer functional programming
- prefer arrow functions with a single options-object parameter
- prefer `createX` naming for factory functions
- prefer return explicitly typed object from functions
- annotate every variable, parameter and return type explicitly
- prefer `type` aliases over `interface`
- prefer `void 0` over `undefined` wherever it appears as a value
- end every file with grouped exports in this order:

```ts
export type {};
export {};
```

These rules are enforced by the code. Violating the letter is violating the spirit.

## When to Use

- creating a new file in a repo
- writing or editing any `.js` / `.ts` / `.jsx` / `.tsx` file
- asked to "match Alpheus style" / "write like Alpheus"

## When NOT to Use

- a repo with its own AGENTS.md that explicitly contradicts a rule below

## Topics

This skill is split across sub-files so agent loads only what they needs.

The rules in this `SKILL.md` apply to every keystroke. For everything else, read the relevant sub-file first.

Sections within each reference run general → specific, declarative → behavioural → exceptional; the most universally-applied rule first, the most situational last.

| Topic             | Reference                                             |
| ----------------- | ----------------------------------------------------- |
| Structure         | [structure.md](./references/structure.md)             |
| Imports & Exports | [imports-exports.md](./references/imports-exports.md) |
| Types             | [types.md](./references/types.md)                     |
| Functions         | [functions.md](./references/functions.md)             |
| Patterns          | [patterns.md](./references/patterns.md)               |
| Comments          | [comments.md](./references/comments.md)               |
| Testing           | [testing.md](./references/testing.md)                 |

| If the task is...               | Read these topics first                 |
| ------------------------------- | --------------------------------------- |
| Creating a new file             | Structure, Functions, Imports & Exports |
| Adding imports / fixing exports | Imports & Exports                       |
| Adding/editing a type           | Types                                   |
| Writing/editing a function      | Functions, Types                        |
| Naming a symbol / file          | Patterns                                |
| Writing JSDoc / comments        | Comments                                |
| Reviewing for anti-patterns     | Patterns                                |
| Writing tests                   | Testing                                 |
