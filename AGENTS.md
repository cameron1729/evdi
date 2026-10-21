# GPU capture prototype context

For work on `gpu-shared-capture`, read [GPU-PROTOTYPE.md](GPU-PROTOTYPE.md) and the
[companion Vino handover](https://github.com/cameron1729/Vino/blob/gpu-encoder-prototype/docs/gpu-handover.md).
If the sibling Vino checkout is available, its local path is
`../Vino/docs/gpu-handover.md`.

The feature branch preserves EVDI v1.15.1 plus copy batching and experimental
shared framebuffer capture. Shared capture has isolated build/private checks;
its combined live compositor trial is still pending. Archived reports do not
establish the currently loaded kernel/module or connected hardware.

Keep module changes synchronized with Vino's `tools/gpu/evdi-copy-batching.patch`,
`evdi-shared-capture.patch` and `evdi-shared-capture.h`. Vino owns the userspace
integration, preparation/recovery tools and host fault harness. Its preparation
script applies both patches to unpatched source, so do not give it this already
patched branch as the input. Update the prototype status when validation changes.
