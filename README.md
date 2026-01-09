## NASA ODTBX: Kalman update order sensitivity (single precision)

### What this is

A runnable Octave script that uses **ODTBX**’s `kalmup` update routine to show a concrete fact:

- if you process multiple scalar measurements sequentially, **the order of those updates can change the final state in single precision** (due to floating point rounding / cancellation).

This is not a claim about GPUs. This is a **numerical determinism boundary** inside a NASA-open-source codebase surface.

### Why it matters

In autonomy / orbit determination pipelines, “same data, different order” can happen through:

- callback timing,
- thread scheduling,
- batching differences,
- sensor drop/retry.

If the estimator boundary is order-sensitive in single precision, then replay/debugging becomes fragile unless the boundary is made deterministic (fixed ordering, stable summation, receipts).

### How to run (Windows)

This expects:

- Octave 10.3.0 installed (winget is fine)
- ODTBX mirror present at:
  - `C:/Users/dammi/third_party/odtbx-mirror/odtbx/ODTBX_Source`

Run:

- `octave-cli.exe --quiet --eval "run('C:/Users/dammi/deal_room/odtbx_order_sensitivity/odtbx_kalmup_order_sensitivity.m')"`

### Outputs

- prints two final `x` states (same inputs, different measurement order)
- prints per-element `single` hex words for a hard “bit-level” comparison

