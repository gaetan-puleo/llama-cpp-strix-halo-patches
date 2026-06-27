# RDNA3.5 Strix Halo llama.cpp patches

Patch set for applying local AMD Strix Halo / RDNA3.5 ROCm tuning to upstream `llama.cpp`.

Base upstream commit:

```text
3fc4e1052 sched : reintroduce less synchronizations during split compute (#20793)
```

Resulting tuned commit:

```text
d9172d620 ggml-cuda: add RDNA3.5 fast prefill path
```

## Patch series

Apply the ordered series with commit history preserved:

```bash
git am -3 /path/to/strix-halo-patches/000*.patch
```

Files:

```text
0001-ggml-cuda-tune-RDNA3.5-matmul-paths.patch
0002-ggml-cuda-apply-RDNA3.5-Strix-Halo-tuning.patch
0003-ggml-cuda-tune-RDNA3.5-MoE-paths.patch
0004-docs-add-Strix-Halo-optimization-notes.patch
0005-ggml-cuda-tune-RDNA3.5-MMVQ-warps.patch
0006-ggml-cuda-tune-RDNA3.5-MoE-prefill.patch
0007-ggml-cuda-add-RDNA3.5-fast-prefill-path.patch
```

## Combined patches

Apply all changes as one commit:

```bash
git apply --index /path/to/strix-halo-patches/strix-halo-rdna35-combined.patch
git commit -m "ggml-cuda: apply RDNA3.5 Strix Halo tuning"
```

Apply code only, excluding `STRIX_HALO_NOTES.md`:

```bash
git apply --index /path/to/strix-halo-patches/strix-halo-rdna35-code-only.patch
git commit -m "ggml-cuda: apply RDNA3.5 Strix Halo tuning"
```

## Notes

- Target hardware: AMD Strix Halo / `gfx1151` / RDNA3.5.
- Target stack used for testing: ROCm 7.2.4.
- Keep runtime `-fa on` for the tested large-context paths.
- Avoid `GGML_HIP_ROCWMMA_FATTN=ON` on `gfx1151`; it regressed FA in local testing.
- Some changes are intentionally aggressive/private performance tuning and are not conservative precision-quality changes.
- No AGPL code was copied into these patches.

## Verification

The patch series was verified locally with:

```bash
git apply --check strix-halo-rdna35-combined.patch
git apply --check strix-halo-rdna35-code-only.patch
git am 000*.patch
```
