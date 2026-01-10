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
