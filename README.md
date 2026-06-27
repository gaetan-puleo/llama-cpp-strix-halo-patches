# llama.cpp Strix Halo patches

AMD Strix Halo / RDNA3.5 ROCm tuning patches for upstream `llama.cpp`.

Target GPU: `gfx1151` / RDNA3.5.

Tested with ROCm 7.2.4.

## Easiest Local Apply

Clone this patch repo next to a fresh `llama.cpp` checkout:

```bash
git clone https://github.com/ggerganov/llama.cpp.git
git clone https://github.com/gaetan-puleo/llama-cpp-strix-halo-patches.git
```

Then apply the patch locally inside `llama.cpp`:

```bash
cd llama.cpp
git checkout 3fc4e1052
git switch -c strix-halo-rdna35
git apply --3way --index ../llama-cpp-strix-halo-patches/strix-halo-rdna35-combined.patch
```

That is the recommended `git apply` path.

## One Command

If you are already inside a clean `llama.cpp` repo and this patch repo is next to it:

```bash
git apply --3way --index ../llama-cpp-strix-halo-patches/strix-halo-rdna35-combined.patch
```

## Keep The Original Commits

The numbered files were generated with `git format-patch`.

Use `git am`, not `git apply`, if you want to keep the original 7 commits:

```bash
git am -3 ../llama-cpp-strix-halo-patches/000*.patch
```

Patch files:

```text
0001-ggml-cuda-tune-RDNA3.5-matmul-paths.patch
0002-ggml-cuda-apply-RDNA3.5-Strix-Halo-tuning.patch
0003-ggml-cuda-tune-RDNA3.5-MoE-paths.patch
0004-docs-add-Strix-Halo-optimization-notes.patch
0005-ggml-cuda-tune-RDNA3.5-MMVQ-warps.patch
0006-ggml-cuda-tune-RDNA3.5-MoE-prefill.patch
0007-ggml-cuda-add-RDNA3.5-fast-prefill-path.patch
```

## Base

These patches were made from this upstream `llama.cpp` commit:

```text
3fc4e1052 sched : reintroduce less synchronizations during split compute (#20793)
```

Final tuned commit in my fork:

```text
d9172d620 ggml-cuda: add RDNA3.5 fast prefill path
```

They may still apply to newer `llama.cpp`, but conflicts are possible.

## Notes

- Keep runtime `-fa on` for the tested large-context paths.
- Avoid `GGML_HIP_ROCWMMA_FATTN=ON` on `gfx1151`; it regressed FA in local testing.
- Some changes are aggressive performance tuning, not conservative precision-quality changes.
- No AGPL code was copied into these patches.
