# Architecture

## Overview

This is anyhow, a Rust crate for ergonomic error handling built around a single anyhow::Error type that can wrap any failure and carry user context. The main entry point is the Macros & Public Facade module, where lib.rs re-exports the public API and the anyhow!/bail!/ensure! macros build ad-hoc errors via the core constructors. At runtime, Error Core & Type Erasure implements Error as a one-word thin pointer to a type-erased payload dispatched through a per-type vtable for drop, clone, and downcast, Diagnostics & Context renders that error with its source chain, backtrace, and attached context, and Compiler-Capability Gating probes the toolchain at build time so the crate works on both stable and nightly Rust.

## Architectural Patterns

- Facade / Single Core Abstraction
- Layered internal architecture
- Type erasure / Boxing pattern
- Decorator/Wrapper pattern (error chains)
- Conditional compilation (feature-gated and version-gated)
- Macro API layer

## Project Context

- **Project Type:** Rust utility/error-handling library
- **Domain:** Systems Programming / Software Infrastructure

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

Defines anyhow::Error as a one-word thin pointer wrapping type-erased error payloads, using a per-type vtable for drop/ref/downcast plus repr(transparent) wrapper types and raw-pointer aliases, and exposes the ErrorKind classification trait. [evidence-linked: 17 call edges]

- Keywords: [`keywords/1.md`](keywords/1.md) — 37 scored symbol(s)

### Diagnostics & Context

Decorates and renders errors by attaching user context to Result/Option, iterating the source-cause chain, capturing backtraces when the underlying error lacks one, and producing the pretty Display/Debug output. [evidence-linked: 14 call edges]

- Keywords: [`keywords/2.md`](keywords/2.md) — 29 scored symbol(s)

### Macros & Public Facade

Provides the public crate surface and ergonomic entry points, with lib.rs declaring internal modules and re-exporting the documented API while anyhow!/bail! and ensure! macros build ad-hoc errors by delegating to the core constructors. [evidence-linked: 3 call edges]

- Keywords: [`keywords/3.md`](keywords/3.md) — 31 scored symbol(s)

### Compiler-Capability Gating

Probes the build environment to detect whether the nightly error_generic_member_access provider API is usable and emits version-dependent cfg flags, keeping the crate compatible across stable/nightly and old rustc versions.

- Keywords: [`keywords/4.md`](keywords/4.md) — 9 scored symbol(s)

### Test Suite & Packaging

Exercises the crate's public behavior end-to-end through the integration test suite and defines the crate's packaging, features, and licensing metadata.

