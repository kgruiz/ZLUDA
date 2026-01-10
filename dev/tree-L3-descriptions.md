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

## dev/
- Overview — Project notes and documentation used for onboarding/explaining ZLUDA internals.
- diagram.md — Mermaid flowchart of ZLUDA’s main execution paths (driver API, PTX compile, library shims).
- explain.md — High-level “three jobs” explanation with call flow pointers.
- explanation.md — Expanded, glossary-style walkthrough of ZLUDA architecture and call flows.
- tree-L3.txt — Raw `tree -L 3` snapshot of the repository.
- tree-L3-descriptions.md — This file: structured descriptions of the tree snapshot.

## docs/
- Overview — mdBook documentation sources and configuration for the ZLUDA docs site.
- book.toml — mdBook configuration (title, authors, source dir).

### docs/src/
- Overview — Markdown content for the docs site.
- building.md — Build prerequisites and basic build commands.
- faq.md — FAQ covering support scope, roadmap, and hardware/software questions.
- quick_start.md — Download/usage instructions for Windows and Linux.
- SUMMARY.md — mdBook table of contents.
- troubleshooting.md — zluda_trace usage, logging, and troubleshooting guide.

## ext/
- Overview — Third-party and FFI “sys” dependencies (detours, ROCm/HIP, HiGHS, LLVM project, etc.).

### ext/detours/
- Overview — Upstream Microsoft Detours source tree for Windows API hooking/instrumentation.
- CREDITS.TXT — Credits and acknowledgements for Detours contributors.
- LICENSE.md — MIT license for Detours.
- Makefile — Make-based build rules for Detours.
- README.md — Detours overview, platform notes, and build guidance.
- system.mak — Shared makefile settings for Detours builds.

#### ext/detours/samples/
- Overview — Sample programs demonstrating Detours usage.

#### ext/detours/src/
- Overview — Detours C/C++ source code.

#### ext/detours/tests/
- Overview — Detours test programs.

#### ext/detours/vc/
- Overview — Visual Studio project files for Detours.

### ext/HiGHS/
- Overview — HiGHS linear optimization solver source (vendored as a submodule).

### ext/highs-sys/
- Overview — Rust sys crate for HiGHS with build/discovery logic.
- build.rs — Builds HiGHS via CMake and sets link flags.
- Cargo.toml — Manifest and feature flags for highs-sys.
- install-dependencies.sh — Helper script to install CMake/stdlib dependencies.
- README.md — Usage and build instructions for highs-sys/HiGHS.
- wrapper.h — C wrapper header for bindgen (HiGHS C API include).

#### ext/highs-sys/src/
- Overview — highs-sys bindings and crate source.

#### ext/highs-sys/tests/
- Overview — highs-sys tests.

### ext/hip_runtime-sys/
- Overview — Raw HIP runtime FFI bindings crate.
- build.rs — Platform-specific linker directives for HIP runtime.
- Cargo.toml — Manifest for hip_runtime-sys.

#### ext/hip_runtime-sys/src/
- Overview — HIP runtime bindings source.

### ext/hipblaslt-sys/
- Overview — Raw hipBLASLt FFI bindings crate.
- build.rs — Linker directives for hipBLASLt.
- Cargo.toml — Manifest for hipblaslt-sys.

#### ext/hipblaslt-sys/src/
- Overview — hipBLASLt bindings source.

### ext/llvm-project/
- Overview — LLVM source tree (vendored as a submodule).

### ext/miopen-sys/
- Overview — Raw MIOpen FFI bindings crate.
- build.rs — Linker directives for MIOpen.
- Cargo.toml — Manifest for miopen-sys.

#### ext/miopen-sys/src/
- Overview — MIOpen bindings source.

### ext/rocblas-sys/
- Overview — Raw rocBLAS FFI bindings crate.
- build.rs — Linker directives for rocBLAS.
- Cargo.toml — Manifest for rocblas-sys.

#### ext/rocblas-sys/src/
- Overview — rocBLAS bindings source.

### ext/rocm_smi-sys/
- Overview — Raw ROCm SMI FFI bindings crate.
- build.rs — Linker directives for ROCm SMI.
- Cargo.toml — Manifest for rocm_smi-sys.

#### ext/rocm_smi-sys/src/
- Overview — ROCm SMI bindings source.

## llvm_zluda/
- Overview — LLVM integration crate; builds/links LLVM via `build.rs` and exposes compile/FFI utilities.

## ptx/
- Overview — PTX translation/processing crate; depends on `ptx_parser`/`llvm_zluda` and includes PTX implementation bitcode.

## zluda/
- Overview — Main ZLUDA crate that builds the `nvcuda` cdylib and houses core CUDA-replacement logic plus OS-specific code.
