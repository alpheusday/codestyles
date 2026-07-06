---
name: alpheus-rust
description: Use when writing or editing Rust code to enforce Alpheus coding style.
---

# Alpheus Coding Style (Rust)

## Overview

- use snake_case directories and files; `mod.rs` in every directory-module
- imports grouped `std` → external → `crate::`, one crate per `use`, blank-line separated
- use `crate::` for all intra-crate paths, never `self::` or `super::`
- prefer to annotate every local explicitly, even when inferable
- prefer to do testing in a separate workspace member
- `.unwrap()` only in tests, avoid in fallible library code

These rules are enforced by the code. Violating the letter is violating the spirit.

## When to Use

- creating a new file in a repo
- writing or editing any `.rs` file
- asked to "match Alpheus style" / "write like Alpheus"

## When NOT to Use

- a repo with its own AGENTS.md that explicitly contradicts a rule below

## Topics

This skill is split across sub-files so the agent loads only what it needs.

The rules in this `SKILL.md` apply to every keystroke. For everything else, read the relevant sub-file first.

Sections within each reference run general → specific, declarative → behavioural → exceptional; the most universally-applied rule first, the most situational last.

| Topic             | Reference                                             |
| ----------------- | ----------------------------------------------------- |
| Structure         | [structure.md](./references/structure.md)             |
| Imports & Exports | [imports-exports.md](./references/imports-exports.md) |
| Types             | [types.md](./references/types.md)                     |
| Functions         | [functions.md](./references/functions.md)             |
| Patterns          | [patterns.md](./references/patterns.md)               |
| Axum              | [axum.md](./references/axum.md)                       |
| Comments          | [comments.md](./references/comments.md)               |
| Testing           | [testing.md](./references/testing.md)                 |

| If the task is...               | Read these topics first                 |
| ------------------------------- | --------------------------------------- |
| Creating a new file / module    | Structure, Functions, Imports & Exports |
| Adding a new feature module     | Structure, Functions                    |
| Adding imports / fixing exports | Imports & Exports                       |
| Adding/editing a type           | Types, Patterns                         |
| Writing/editing a function      | Functions                               |
| Writing a handler / service     | Axum, Functions, Patterns               |
| Writing a builder               | Functions, Types                        |
| Writing a proc-macro            | Types, Patterns, Imports & Exports      |
| Naming a symbol / file          | Patterns, Structure                     |
| Writing doc comments            | Comments                                |
| Writing tests                   | Testing                                 |
| Reviewing for anti-patterns     | Patterns                                |
