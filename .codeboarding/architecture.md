# Architecture

## Overview

This is the anyhow crate for ergonomic Rust error handling, with its user-facing entry point in Public API, Macros & Context: the anyhow!/bail!/ensure! macros and the Context trait that convert Results and Options into anyhow::Error values. At runtime, Error Core & Type Erasure implements the Error type itself using an ErrorVTable and Own/Ref/Mut pointer abstractions that keep it exactly one word wide while supporting downcasting and std::error::Error adapters for non-error payloads. Diagnostics & Backtrace then traverses a chain of source() errors to render "Caused by:" reports with backtraces, while Toolchain & Provider Probe detects compiler capabilities at build time so backtrace capture and forwarding through core::error::Request can be conditionally enabled.

## Architectural Patterns

- Facade Pattern (lib.rs aggregates and re-exports the public
- Layered Architecture (API layer: macros and public types;
- Type Erasure / Opaque Handle (ptr.rs/wrapper.rs store any
- Decorator/Chain of Responsibility variant (context.rs + chain.rs implement
- Capability Gating (nightly.rs + build.rs implement conditional compilation
- Utility/Helper Library pattern (zero services, zero runtime, zero

## Project Context

- **Project Type:** Rust Library
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

### Public API, Macros & Context

Defines the crate's user-facing surface: the re-exports and cfg plumbing in lib.rs, the anyhow!/bail!/ensure! macros (including the __parse_ensure TT-muncher), and the Context trait's context()/with_context() for Result/Option with its ContextError wrapper. [evidence-linked: 9 call edges]

- Keywords: [`keywords/1.md`](keywords/1.md) — 43 scored symbol(s)

### Error Core & Type Erasure

Implements anyhow::Error itself: the ErrorVTable type-erasure and ErrorImpl representation, the Own/Ref/Mut pointer abstractions that make Error exactly one word wide, constructors/downcasting/conversions, the autoref-based Kind dispatch that lets anyhow!(expr) pick the right constructor, and the repr(transparent) adapter wrappers (MessageError, DisplayError, BoxedError) lending std::error::Error semantics to non-error payloads. [evidence-linked: 17 call edges]

- Keywords: [`keywords/2.md`](keywords/2.md) — 37 scored symbol(s)

### Diagnostics & Backtrace

Traverses and renders errors: the Chain double-ended exact-size iterator over a dyn Error's source() chain, the Caused by:/numbered-indentation/alternate-Debug report formatting, and the cfg-gated std::backtrace re-exports plus the backtrace!/backtrace_if_absent! macros. [evidence-linked: 8 call edges]

- Keywords: [`keywords/3.md`](keywords/3.md) — 17 scored symbol(s)

### Toolchain & Provider Probe

Probes the compiler and emits the anyhow_*/error_generic_member_access cfg flags consumed throughout src/, bridges to core::error::Request/provide so a backtrace can be read or forwarded through an underlying error's own provide impl, and pins the compiler version.

- Keywords: [`keywords/4.md`](keywords/4.md) — 9 scored symbol(s)

