1. Export the CUDA Driver API (the “front door”)

* What it is: a shared library that exports the same CUDA Driver API function symbols (exported function names the program links to) that NVIDIA normally provides in `nvcuda.dll` (Windows) or `libcuda.so` (Linux).
* What “same” means: same public API contract (function names like `cuInit`, `cuMemAlloc_v2`, `cuModuleLoadData`, `cuLaunchKernel`; compatible signatures and calling convention/ABI; expected return codes and behavior). It is not NVIDIA’s implementation - it is ZLUDA’s Rust implementation that tries to match NVIDIA’s outward behavior.
* Where we pointed in the repo: `zluda/src/lib.rs` is the export surface; it declares lots of CUDA Driver API symbols using macros (the idea was “macros generate repetitive wrappers”). The real logic lives under `zluda/src/impl/*` split by topic (context/device/memory/module/function/kernel/stream/etc.).
* How it loads: you asked whether it uses the same filenames as NVIDIA. For a true drop-in, yes, it generally must either (a) use the same library filename the app loads (`nvcuda.dll` / `libcuda.so`) or (b) use redirect/injection/loader tricks so the app still loads the NVIDIA name but calls into ZLUDA. We referenced the repo’s OS-specific pieces for that: Windows (`zluda_inject`, `zluda_redirect`, `detours-sys`) and Linux (`zluda_ld` with ld-audit redirection).
* Key terms we defined:

  * DLL/SO: the loadable library file (`.dll` Windows, `.so` Linux).
  * Dynamic linking: app loads symbols from that library at runtime.
  * Symbol: exported function name like `cuInit`.
  * ABI: the binary calling convention the app expects.

2. Compile and run CUDA kernels (PTX → LLVM → backend) (the “compiler heart”)

* What it is: the part that makes actual GPU kernels run. CUDA apps often provide kernels in PTX (NVIDIA’s GPU assembly-like format). ZLUDA cannot run PTX directly on AMD; it has to translate and compile it for the AMD stack.
* When it happens: not “always at startup.” It’s triggered by specific driver calls, mainly when the app loads a module and later launches a kernel. We discussed it as being invoked when the app calls things like `cuModuleLoadData` and then `cuLaunchKernel`.
* The concrete call flow we described:

  * `cuModuleLoadData` (driver API) → `zluda/src/impl/module.rs::load_data`
  * inspect the blob via something like `CodeLibraryRef::try_load` (detect PTX vs fatbin vs ELF)
  * parse PTX via `ptx_parser::parse_module_checked`
  * pick “best PTX” if multiple candidates exist
  * build a compile cache key (hash like `blake3` + device arch + ZLUDA version + compile attributes)
  * on cache miss: lower PTX to LLVM IR via `ptx/src/pass/mod.rs` (the “pass pipeline”)
  * compile via `llvm_zluda::compile` to a backend-loadable binary (the excerpt described ELF output)
  * load into HIP via `hipModuleLoadData`
  * then for launching:

    * `cuModuleGetFunction` → `hipModuleGetFunction`
    * `cuLaunchKernel` → `hipModuleLaunchKernel` using grid/block dimensions
* Where it lives: `ptx_parser`, `ptx/src/pass/mod.rs`, `llvm_zluda/*`, and the driver-side glue in `zluda/src/impl/module.rs` and `zluda/src/impl/function.rs`.
* Key terms we defined:

  * Kernel: GPU function launched over many threads.
  * PTX: NVIDIA’s kernel format.
  * LLVM IR: compiler intermediate form.
  * Pass/lowering: stepwise transforms to make code compatible.
  * Grid/block: CUDA launch geometry (how many blocks, how many threads per block).
  * Cache key / cache miss: how compiled results are reused.

3. Shim CUDA libraries (cuBLAS, cuDNN, cuFFT, cuSPARSE, NVML) (the “drop-in libraries”)

* What it is: optional replacement libraries for CUDA’s ecosystem libraries beyond the driver. Many real apps (especially ML frameworks) do not just call the driver API - they load cuBLAS/cuDNN/etc. If those libraries are missing, the app fails even if kernel launch works.
* How it works: each shim exports the same library API as NVIDIA’s library, but inside it forwards or translates calls to AMD equivalents. This is “API compatibility via mapping,” not “bit-for-bit same implementation.”
* Concrete examples from the repo:

  * `zluda_blas`: cuBLAS → rocBLAS
  * `zluda_dnn` + `zluda_dnn8` / `zluda_dnn9`: cuDNN → MIOpen
  * `zluda_ml`: NVML → ROCm SMI on Linux; Windows implementation is largely unimplemented
  * `zluda_fft`: cuFFT shim exists but functions are currently unimplemented
  * `zluda_sparse`: cuSPARSE shim exists but most functions are currently unimplemented
  * plus the raw FFI bindings under `ext/*-sys` like `ext/hip_runtime-sys`, `ext/rocblas-sys`, `ext/miopen-sys`, `ext/rocm_smi-sys`
* When it matters (your sequencing question): it is not a required step for every program. It is “needed if the program uses those libraries.” The driver API layer is the entrypoint; kernel compilation is needed for kernel execution; shims are needed for higher-level library calls.
* Key terms we defined:

  * Shim: wrapper that mimics one API and calls another implementation.
  * FFI: Rust calling C libraries.
  * rocBLAS/MIOpen/ROCm SMI: AMD equivalents to cuBLAS/cuDNN/NVML.

How these 3 relate (based on your earlier question)

* They are not three separate “ways” you pick from. They are components that combine:

  * The exported Driver API is the always-on entry layer.
  * Some Driver API calls trigger the compiler path (module load, kernel launch).
  * If the app also depends on cuBLAS/cuDNN/etc., it will load those separate libraries, so you need shims too.
