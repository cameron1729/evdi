# Experimental GPU capture branch

This branch preserves the EVDI changes developed alongside
[Vino's `gpu-encoder-prototype` branch](https://github.com/cameron1729/Vino/tree/gpu-encoder-prototype).
The [Vino handover](https://github.com/cameron1729/Vino/blob/gpu-encoder-prototype/docs/gpu-handover.md)
records the current integration status and next work. Its EVDI base is upstream `v1.15.1`, commit
`33b7b3c9f7c77157c2b1b0c5a40cebe19b8ba110`. All 41 module source files at
that base matched the installed package and the isolated build input exactly.

The changes are kept as two commits:

- **Batch full-width capture copies:** opt-in bounded row batching through
  `copy_batch_rows`; zero preserves the existing copy path.
- **Add experimental shared framebuffer capture:** lease the compositor's
  imported framebuffer through a DMA-buf proxy so Vino can copy it on the GPU.

Shared capture is an experimental ABI, not an upstream interface. It is scoped
to the supervised AMD/Xorg implicit-sync prototype, linear XRGB8888, separate
cursor events and one 1920x1080 display. The source framebuffer keeps a producer
exclusion fence while the proxy exposes its retained system-memory pages with a
separate reservation object. Revocation invalidates the capture without releasing
a buffer that GPU work may still read. A hung consumer can require termination
by the independent trial supervisor.

## Preserved validation

The final module sources match Vino's isolated `prepared-proxy/after` build
byte for byte. The original and candidate kernel builds passed on
`7.0.0-34-generic`, along with 47 host fault-injection and lifetime checks under
ASan and UBSan. The candidate source version was `9F72611562082A8AFD64D95`.

Copy batching has had live desktop trials. The combined shared-framebuffer
capture path has **not** had a live compositor trial and is not production ready.
Private DMA-buf and GPU comparison checks do not establish live recovery safety.

The companion Vino checkout retains the integration, build/recovery tools,
fault harness and reports:

- `tools/gpu/evdi-copy-batching.patch`
- `tools/gpu/evdi-shared-capture.patch`
- `tools/gpu/evdi-shared-capture-test.c`
- `tools/gpu/prepare-evdi-shared.py`
- `tools/gpu/evdi-module-trial.py`
- `docs/gpu-shared-capture.md`
- `docs/results/gpu-shared-capture-20261021/kernel-build-report.json`

This branch contains source only. Creating it does not install a module or
change the active display stack.
