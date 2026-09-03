# Architecture

## Overview

This is the anyhow error handling crate for Rust, whose main entry point is the anyhow::Error/anyhow::Result type that accepts any std error, ad-hoc message, or boxed trait object. Error Core & Type Erasure backs that type with a hand-rolled vtable yielding a one-word thin pointer with downcasting, while Backtrace & Nightly Interop captures a std::backtrace::Backtrace when the underlying error lacks one and bridges the nightly provide/request_ref API. The rest of the runtime is split among Context, Chains & Rendering for attaching user context, iterating source chains, and printing the 'Error: … / Caused by: …' output, Macros & Tagged Dispatch for anyhow!/bail!/ensure! ergonomics via autoref specialization emulation, and Build & Toolchain Detection for compiler probing and feature gating.

## Architectural Patterns

- Facade / single-crate API surface
- Type erasure via trait objects
- Flat layered module organization
- Decorator/context-attachment pattern
- Macro layer over runtime layer
- Conditional compilation boundary
- Adaptor/shim pattern

## Project Context

- **Project Type:** Rust error-handling foundation library
- **Domain:** Systems programming / Software infrastructure

## Tech Stack

_No tech stack detected._

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

### Error Core & Type Erasure

Implements the anyhow::Error/anyhow::Result types with construction from any std error, ad-hoc message, or boxed trait object, plus downcasting and conversion via a hand-rolled vtable giving a one-word thin pointer. [evidence-linked: 19 call edges]

- Keywords: [`keywords/1.md`](keywords/1.md) — 38 scored symbol(s)

### Backtrace & Nightly Interop

Captures a std::backtrace::Backtrace when the underlying error lacks one via the backtrace!/backtrace_if_absent! macros and bridges the nightly error_generic_member_access provide/request_ref API. [evidence-linked: 1 call edges]

- Keywords: [`keywords/2.md`](keywords/2.md) — 23 scored symbol(s)

### Context, Chains & Rendering

Attaches user context to Result/Option via the Context trait, iterates source-cause chains, and renders the human-readable 'Error: … / Caused by: …' Debug/Display output. [evidence-linked: 14 call edges]

- Keywords: [`keywords/3.md`](keywords/3.md) — 24 scored symbol(s)

### Macros & Tagged Dispatch

Provides user-facing ergonomics (anyhow!, bail!, ensure!) and the autoref tagged dispatch (AdhocKind/TraitKind/BoxedKind) that emulates specialization to decide how anyhow!($expr) is converted. [evidence-linked: 4 call edges]

- Keywords: [`keywords/4.md`](keywords/4.md) — 30 scored symbol(s)

### Build & Toolchain Detection

Build script that probes the compiler (compiling src/nightly.rs) to enable error_generic_member_access, emits version-gated cfgs, and includes the crate/feature manifest and toolchain pin.

- Keywords: [`keywords/5.md`](keywords/5.md) — 4 scored symbol(s)

