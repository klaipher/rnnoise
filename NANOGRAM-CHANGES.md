# Nanogram changes to xiph/rnnoise

Branch `nanogram` = xiph/rnnoise `904a876` (tag `v0.2`, the "little" model) plus three commits. The Nanogram iOS app
carried the same delta in its vendored `third-party/rnnoise`; iOS, macOS and Android now build this branch. Android
used a 2018 copy of rnnoise before (a different model and API); moving to 0.2 changes its noise suppression output.

| Commit | Change | Effect |
| --- | --- | --- |
| `2b9202c` | Portability headers | `OPUS_GNUC_PREREQ`, `RNN_CLEAR` and local includes in `common.h`, `vec.h`, `vec_avx.h`, `vec_neon.h`, `x86/x86cpu.h`, so the library builds without Opus' `celt`/`silk` tree. No change to the processing. |
| `8e6520a` | Model loading hardening | `src/denoise.c`: `rnnoise_model_from_filename` returns NULL for a missing file instead of crashing; `model_from_file` rejects negative and oversized lengths and initialises the blob pointers. |
| `d2d7997` | Built-in model | `src/rnnoise_data.c` and `.h`: the compiled-in weights of the "little" model, 4.9 MB, at reduced float precision (the `.h` is identical to upstream `0b50c45`). Without this file the library has no compiled-in weights. The reduced precision is the table the iOS app has shipped. |

## Upstream status

Not submitted upstream. The model table is generated, not hand-edited.
