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

## cuda_base/
- Overview — Directory present in the tree snapshot with no tracked files at depth 3.

## cuda_macros/
- Overview — Proc-macro crate that generates CUDA-family extern declarations and normalizes API naming.
- Cargo.toml — Manifest for the proc-macro crate and its parsing/quote dependencies.

### cuda_macros/build/
- Overview — Wrapper headers used for generating bindings.
- wrapper.h — Aggregates CUDA/VDPAU headers and enables internal API versioning for binding generation.

### cuda_macros/src/
- Overview — Proc-macro implementation plus generated extern declarations.
- cublas.rs — Generated extern declarations for cuBLAS functions.
- cublaslt.rs — Generated extern declarations for cuBLASLt functions.
- cublaslt_internal.rs — Bindgen-generated extern declarations for internal cuBLASLt functions.
- cuda.rs — Generated extern declarations for the CUDA driver API.
- cudnn8.rs — Generated extern declarations for cuDNN v8 functions.
- cudnn9.rs — Generated extern declarations for cuDNN v9 functions.
- cufft.rs — Generated extern declarations for cuFFT functions.
- cusparse.rs — Generated extern declarations for cuSPARSE functions.
- lib.rs — Proc-macro definitions for choosing overrides, normalizing names, and a test helper macro.
- nvml.rs — Generated extern declarations for NVML functions.

## cuda_types/
- Overview — CUDA/ROCm type and API bindings used across the project.
- Cargo.toml — Manifest for CUDA type definitions and ROCm/HIP sys dependencies.

### cuda_types/src/
- Overview — Module definitions for CUDA library types and FFI structs.
- cublas.rs — Generated CUDA cuBLAS type/FFI definitions.
- cublaslt.rs — Generated CUDA cuBLASLt type/FFI definitions.
- cuda.rs — Generated CUDA driver API types and constants.
- cudnn.rs — Generated CUDA cuDNN type/FFI definitions (generic layer).
- cudnn8.rs — Generated CUDA cuDNN v8 type/FFI definitions.
- cudnn9.rs — Generated CUDA cuDNN v9 type/FFI definitions.
- cufft.rs — Generated CUDA cuFFT type/FFI definitions.
- cusparse.rs — Generated CUDA cuSPARSE type/FFI definitions.
- dark_api.rs — Handwritten structs/flags for CUDA fatbin headers and related internal formats.
- lib.rs — Module exports for the CUDA type/FFI surface.
- nvml.rs — Generated NVML type/FFI definitions.

## dark_api/
- Overview — Internal/undocumented CUDA “dark API” interfaces and fatbin tooling.
- Cargo.toml — Manifest for the dark_api crate and its compression/format dependencies.

### dark_api/src/
- Overview — Implementation of dark API tables and fatbin parsing helpers.
- fatbin.rs — High-level parser for fatbin wrappers/submodules with decompression helpers.
- lib.rs — Macro-driven dark API tables, GUID mapping, formatting helpers, and integrity-check logic.

## detours-sys/
- Overview — Rust FFI bindings to Microsoft Detours (Windows-only hooking/injection).
- Cargo.toml — Manifest and crate metadata for detours-sys.
- LICENSE-APACHE — Apache 2.0 license text for detours-sys.
- LICENSE-MIT — MIT license text for detours-sys.
- README.md — Usage notes and licensing for detours-sys.
- build.rs — Windows-only build script compiling Detours C++ sources from `ext/detours`.

### detours-sys/build/
- Overview — Wrapper headers used for bindgen/build.
- wrapper.h — Includes Windows and Detours headers for binding generation.

### detours-sys/src/
- Overview — Generated bindings and crate entrypoint.
- bundled_bindings.rs — Pre-generated bindgen output for Detours APIs.
- lib.rs — Windows-only crate entry that includes bundled bindings and a test hook example.

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
