# ZLUDA tree (L3) descriptions

- Cargo.toml — Workspace manifest listing all member crates, default members, custom profiles, and a local patch for `highs-sys`.
- Cargo.lock — Cargo lockfile pinning exact dependency versions for reproducible builds.
- README.md — Project overview and entry points (quick start, Discord, news links).
- LICENSE-APACHE — Apache 2.0 license text for the project.
- LICENSE-MIT — MIT license text for the project.
- zluda/ — Main ZLUDA crate that builds the `nvcuda` cdylib and houses core CUDA-replacement logic plus OS-specific code.
- llvm_zluda/ — LLVM integration crate; builds/links LLVM via `build.rs` and exposes compile/FFI utilities.
- ptx/ — PTX translation/processing crate; depends on `ptx_parser`/`llvm_zluda` and includes PTX implementation bitcode.
- ext/ — Third-party and FFI “sys” dependencies (detours, ROCm/HIP, HiGHS, LLVM project, etc.).
- docs/ — mdBook documentation sources and configuration for the ZLUDA docs site.
- compiler/ — Offline PTX-to-LLVM compiler crate (the `zoc` binary).
- compiler/Cargo.toml — Manifest for the offline compiler binary and its dependencies.
- compiler/src/ — Source directory for the compiler crate.
- compiler/src/error.rs — Compiler-specific error type and conversions from PTX/parser errors.
- compiler/src/main.rs — CLI entry point that parses options, reads PTX, generates LLVM/bitcode, and invokes `llvm_zluda` compilation (with optional HIP arch detection).
