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

## format/
- Overview — Formatting helpers for pretty-printing CUDA types and generated API arguments.
- Cargo.toml — Manifest for the format crate.

### format/src/
- Overview — CudaDisplay trait implementations and generated formatters.
- dark_api.rs — CudaDisplay implementations for dark API fatbin structs.
- dnn8.rs — cuDNN v8 formatting glue that re-exports generated formatters.
- dnn9.rs — cuDNN v9 formatting helpers with typed element printing.
- format_generated.rs — Generated formatters for core CUDA types.
- format_generated_blas.rs — Generated cuBLAS argument formatters.
- format_generated_blaslt.rs — Generated cuBLASLt argument formatters.
- format_generated_blaslt_internal.rs — Generated formatters for internal cuBLASLt APIs.
- format_generated_dnn8.rs — Generated cuDNN v8 argument formatters.
- format_generated_dnn9.rs — Generated cuDNN v9 argument formatters.
- format_generated_fft.rs — Generated cuFFT argument formatters.
- format_generated_nvml.rs — Generated NVML argument formatters.
- format_generated_sparse.rs — Generated cuSPARSE argument formatters.
- lib.rs — Core CudaDisplay trait and formatting implementations for common CUDA types.

## geekbench.svg
- Overview — SVG asset (likely a benchmark graphic) stored at repo root.

## llvm_zluda/
- Overview — LLVM integration crate; builds/links LLVM via `build.rs` and exposes compile/FFI utilities.
- build.rs — Builds and links the LLVM/LLD toolchain via CMake and configures link flags.
- Cargo.toml — Manifest for the LLVM integration crate.

### llvm_zluda/src/
- Overview — LLVM wrapper modules, compiler pipeline, and FFI helpers.
- compile.rs — Links PTX/OCML/OCKL bitcode, runs LLVM passes, and emits AMDGPU objects/ELF via LLD.
- ffi.rs — Extra LLVM C-API extensions and helpers used by ZLUDA.
- lib.cpp — C++ shims compiled into `llvm_zluda_cpp` for LLVM extensions.
- lib.rs — Crate entry that re-exports LLVM sys bindings and modules.
- utils.rs — Safe wrappers around LLVM context/module/target machine and object utilities.

## ptx/
- Overview — PTX translation/processing crate; depends on `ptx_parser`/`llvm_zluda` and includes PTX implementation bitcode.
- Cargo.toml — Manifest for the PTX translation crate and its dependencies.

### ptx/lib/
- Overview — Bundled PTX implementation artifacts.
- zluda_ptx_impl.bc — Prebuilt LLVM bitcode for PTX helper implementations.
- zluda_ptx_impl.cpp — C++ source for the PTX helper bitcode.

### ptx/src/
- Overview — PTX lowering pipeline and tests.
- lib.rs — Exports the PTX-to-LLVM entry points and types.

#### ptx/src/pass/
- Overview — PTX lowering/normalization passes and LLVM emission.
- deparamize_functions.rs — Rewrites kernel parameters into explicit function args.
- expand_operands.rs — Expands complex operands into simpler forms.
- fix_special_registers.rs — Normalizes PTX special registers into helper calls.
- hoist_globals.rs — Moves globals into a consistent module layout.
- insert_explicit_load_store.rs — Makes memory operations explicit.
- insert_implicit_conversions.rs — Inserts required type conversions.
- insert_post_saturation.rs — Applies post-saturation logic where needed.
- mod.rs — Pass orchestration and `to_llvm_module` pipeline.
- normalize_basic_blocks.rs — Normalizes basic block structure.
- normalize_identifiers.rs — Normalizes identifiers for consistent naming.
- normalize_predicates.rs — Normalizes predicate usage.
- remove_unreachable_basic_blocks.rs — Prunes unreachable control flow.
- replace_instructions_with_functions.rs — Lowers PTX instructions into helper calls.
- replace_instructions_with_functions_fp_required.rs — Lowers instructions requiring FP helpers.
- replace_known_functions.rs — Replaces known function patterns.
- resolve_function_pointers.rs — Resolves PTX function pointer usage.

##### ptx/src/pass/instruction_mode_to_global_mode/
- Overview — Pass and fixtures for converting instruction mode to global mode.
- mod.rs — Instruction-mode to global-mode transformation logic.
- test.rs — Tests for instruction-mode conversion.

##### ptx/src/pass/llvm/
- Overview — LLVM emission utilities.
- attributes.rs — Builds LLVM IR for GPU attributes.
- emit.rs — Main PTX-to-LLVM IR emission logic.
- mod.rs — Module wiring for LLVM emission.

##### ptx/src/pass/test/
- Overview — Test helpers for pass pipeline.

#### ptx/src/test/
- Overview — PTX samples and expected outputs for testing.
- _Z9vectorAddPKfS0_Pfi.ptx — PTX fixture for vector add kernel.
- mod.rs — Test harness for PTX fixtures.
- operands.ptx — PTX fixture covering operand handling.
- vectorAdd_11.ptx — PTX fixture for vector add variant.
- vectorAdd_kernel64.ptx — PTX fixture for 64-bit kernel variant.

##### ptx/src/test/ll/
- Overview — Expected LLVM IR outputs for tests.

##### ptx/src/test/spirv_build/
- Overview — SPIR-V build fixtures used in tests.

##### ptx/src/test/spirv_fail/
- Overview — SPIR-V failure fixtures used in tests.

##### ptx/src/test/spirv_run/
- Overview — SPIR-V run fixtures used in tests.

## ptx_parser/
- Overview — PTX parser crate that tokenizes and parses PTX into an AST.
- Cargo.toml — Manifest for the PTX parser crate.

### ptx_parser/src/
- Overview — Parser implementation, AST definitions, and helpers.
- ast.rs — PTX AST types and instruction definitions.
- check_args.py — Helper script for validating parser argument definitions.
- lib.rs — Core lexer/parser logic and PTX parsing entry points.

## ptx_parser_macros/
- Overview — Proc-macro crate for generating PTX instruction types and parsers.
- Cargo.toml — Manifest for the parser macros crate.

### ptx_parser_macros/src/
- Overview — Macro implementations for opcode and instruction generation.
- lib.rs — Main proc-macro logic and generation helpers.

## ptx_parser_macros_impl/
- Overview — Shared implementation crate for parser macro codegen.
- Cargo.toml — Manifest for the parser macros implementation crate.

### ptx_parser_macros_impl/src/
- Overview — Parser/codegen helpers used by the proc-macro front end.
- lib.rs — Code generation for instruction types, visitors, and display logic.
- parser.rs — Syn-based parser for the opcode-definition DSL.

## ptxas/
- Overview — Minimal stub of the CUDA `ptxas` tool, currently copying input to output.
- Cargo.toml — Manifest for the ptxas binary crate.

### ptxas/src/
- Overview — ptxas CLI implementation.
- main.rs — CLI parser and passthrough implementation.

## xtask/
- Overview — Build orchestration tool for compiling and packaging ZLUDA artifacts.
- Cargo.toml — Manifest for the xtask helper binary.

### xtask/src/
- Overview — Build/packaging logic for workspace outputs.
- main.rs — CLI for build/zip commands, profiles, and packaging logic for Windows/Linux.

## zluda/
- Overview — Main ZLUDA crate that builds the `nvcuda` cdylib and houses core CUDA-replacement logic plus OS-specific code.
