# ZLUDA tree (L3) descriptions

## Root
- Cargo.toml — Workspace manifest listing all member crates, default members, custom profiles, and a local patch for `highs-sys`.
- Cargo.lock — Cargo lockfile pinning exact dependency versions for reproducible builds.
- README.md — Project overview and entry points (quick start, Discord, news links).
- LICENSE-APACHE — Apache 2.0 license text for the project.
- LICENSE-MIT — MIT license text for the project.

## compiler/
- Overview — Offline PTX-to-LLVM compiler crate (the `zoc` binary).
- Cargo.toml — Manifest for the offline compiler binary and its dependencies.

### compiler/src/
- Overview — Source directory for the compiler crate.
- error.rs — Compiler-specific error type and conversions from PTX/parser errors.
- main.rs — CLI entry point that parses options, reads PTX, generates LLVM/bitcode, and invokes `llvm_zluda` compilation (with optional HIP arch detection).

## docs/
- Overview — mdBook documentation sources and configuration for the ZLUDA docs site.

## ext/
- Overview — Third-party and FFI “sys” dependencies (detours, ROCm/HIP, HiGHS, LLVM project, etc.).

## llvm_zluda/
- Overview — LLVM integration crate; builds/links LLVM via `build.rs` and exposes compile/FFI utilities.

## ptx/
- Overview — PTX translation/processing crate; depends on `ptx_parser`/`llvm_zluda` and includes PTX implementation bitcode.

## zluda/
- Overview — Main ZLUDA crate that builds the `nvcuda` cdylib and houses core CUDA-replacement logic plus OS-specific code.
