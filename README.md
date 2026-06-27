# llama.cpp Strix Halo patches

AMD Strix Halo / RDNA3.5 ROCm tuning patches for upstream `llama.cpp`.

Target GPU: `gfx1151` / RDNA3.5.

Tested with ROCm 7.2.4.

## Apply Locally

Clone `llama.cpp` and this patch repo next to each other:

```bash
git clone https://github.com/ggerganov/llama.cpp.git
git clone https://github.com/gaetan-puleo/llama-cpp-strix-halo-patches.git
```

Apply the combined patch inside `llama.cpp`:

```bash
cd llama.cpp
git apply --3way --index ../llama-cpp-strix-halo-patches/strix-halo-rdna35-combined.patch
```

If you are already inside a clean `llama.cpp` checkout and the patch repo is next to it:

```bash
git apply --3way --index ../llama-cpp-strix-halo-patches/strix-halo-rdna35-combined.patch
```

## Keep Commit History

The numbered patch file is generated with `git format-patch`.

Use `git am`, not `git apply`, if you want to apply it as a commit:

```bash
git am -3 ../llama-cpp-strix-halo-patches/000*.patch
```

## Files

```text
strix-halo-rdna35-combined.patch
000*.patch
```

## Automatic Refresh

GitHub Actions regenerates the patches every 12 hours and can also be run manually.

Source repos:

```text
upstream: ggerganov/llama.cpp:master
fork:     gaetan-puleo/llama-cpp-strix-halo:main
```

The workflow:

```text
1. checks out latest upstream master
2. fetches the fork branch
3. cherry-picks fork-only commits onto upstream master
4. squashes the final tree into one generated patch commit
5. regenerates the combined git-apply patch
6. regenerates the git-format-patch file
7. verifies git apply and git am
8. pushes only if generated files changed
```

If upstream changes conflict with the fork patches, the workflow fails and keeps the last working patch set.

## Notes

- Keep runtime `-fa on` for the tested large-context paths.
- Avoid `GGML_HIP_ROCWMMA_FATTN=ON` on `gfx1151`; it regressed FA in local testing.
- No AGPL code was copied into these patches.
