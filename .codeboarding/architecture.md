# Architecture

## Overview

This is a Rust error-handling library built around its Report type, defined in Core Report Type & Internals, which classifies error kinds, traverses source chains, and stores boxed error trait objects behind thin pointers. The main entry point for users is Context, Macros & Assertions, which attaches contextual messages to Results and provides bail-style early-return and assertion macros that fail into a report. At runtime, errors flow from those macros and context wrappers into Report Formatting & Backtrace Integration, which renders user-facing output and captures backtraces via the nightly-gated provider API, while Build, Toolchain & Integration Tests supplies the build script, manifest, pinned toolchain, and end-to-end test coverage.

## Architectural Patterns

- Type-erased container (dynamic dispatch)
- Facade / flat module library
- Error chain / context accumulation
- Macro facade (ergonomic API surface)
- Conditional implementation (strategy-by-compile-target)
- Downcast/recovery pattern

## Project Context

- **Project Type:** Foundal library
- **Domain:** Systems programming / Rust ecosystem infrastructure

## Tech Stack

`Integration tests in tests/`

## Common Commands

### Build / Install

```bash
cargo build
```

### Test

```bash
cargo test
```

## Key Entry Points

_No standard entry points detected._

## Modules

_Each module links to a per-module keyword file listing its native symbols (file/function/class names kept verbatim for exact grep), ranked by importance. The exact formula depends on the module's graph density: dense graphs use `0.30·bridge + 0.30·usage + 0.15·type + 0.15·activity + 0.10·exported`; sparse graphs (calls hidden behind runtime dispatch) use `0.20·bridge + 0.20·usage + 0.15·type + 0.15·activity + 0.15·exported + 0.15·file_hub`. See each keyword file's header for the rule that produced its scores. Agents read a module's keyword file on demand._

### Core Report Type & Internals

Defines the library's primary report/error type with its internal kind/state classification, source-chain traversal, and low-level unsafe pointer helpers for storing boxed dyn Error trait objects behind thin pointers. [evidence-linked: 19 call edges]

- Keywords: [`keywords/1.md`](keywords/1.md) — 56 scored symbol(s)

### Context, Macros & Assertions

Attaches contextual messages to Results via a context trait and a wrapper error type pairing a context message with an inner error, plus the declarative macro surface of the crate: bail-style early-return macros and an assertion-style macro that fails into a report. [evidence-linked: 11 call edges]

- Keywords: [`keywords/2.md`](keywords/2.md) — 31 scored symbol(s)

### Report Formatting & Backtrace Integration

Renders reports for user-facing output through the default formatting pipeline, captures backtraces, and holds the nightly-gated error provider/request API used for backtrace plumbing. [evidence-linked: 8 call edges]

- Keywords: [`keywords/3.md`](keywords/3.md) — 15 scored symbol(s)

### Build, Toolchain & Integration Tests

Configures the build script, package manifest, and pinned toolchain for the crate, and hosts the end-to-end test suite exercising the public crate surface.

- Keywords: [`keywords/4.md`](keywords/4.md) — 4 scored symbol(s)

