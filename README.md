<div align="center">

<img src="docs/assets/dlss-native-mark.svg" width="92" alt="DLSS Native logo">

# DLSS Native

### Native RTX video processing for Super Resolution, Frame Generation and HDR-aware workflows.

**Windows · NVIDIA RTX · D3D12 · Unified ABI4**

[Project page](https://z13321812367-sys.github.io/DLSS-Native/) · [中文](README.zh-CN.md) · [Third-party notices](THIRD_PARTY_NOTICES.md)

</div>

---

## Overview

DLSS Native is a Windows video-processing project built around a single RTX/D3D12 execution path. It keeps source timing, GPU processing and output scheduling in one pipeline instead of treating each stage as a separate conversion step.

The current product path combines DLSS Super Resolution, Frame Generation, optical-flow guidance and HDR-aware processing under the Unified ABI4 runtime.

```text
Media
  → decode + source timing
  → shared D3D12 / RTX context
  → Super Resolution
  → HDR processing when required
  → Frame Generation
  → deterministic output timeline
  → encode / mux / verify
```

## What matters

### Native GPU ownership

The runtime owns the selected adapter, D3D12 device, queue, feature sessions, optical-flow session and shared surfaces as one sequence-scoped context. Neural stages can exchange GPU surfaces without turning every stage into a CPU round trip.

### Correct video timing

Frame PTS and decoded frame reality drive scheduling. The pipeline does not infer media timing from an average frame rate when the source already provides timestamps.

### HDR as image data

HDR handling keeps precision and transfer semantics in the processing contract. Color metadata alone is not treated as proof that an HDR path is correct.

### Predictable output

Encoding, reordering, muxing and end-of-stream handling are verified as part of the video path, so output correctness is measured at the file boundary rather than assumed from a successful GPU call.

## Current status

| Area | Status |
| --- | --- |
| DLSS Super Resolution | **Available in the current product path** |
| DLSS Frame Generation | **Available in the current product path** |
| Unified ABI4 / D3D12 runtime | **Current architecture** |
| HDR-aware video pipeline | **Current architecture** |
| Neural Rendering / NR | **Runtime-dependent; disabled when a trusted runtime is unavailable** |
| Direct D3D12 → NVENC path | **Active performance validation** |
| Public source / binaries | **Not published yet** |

The integrated product baseline has been validated on an RTX 5060 Ti. Broader performance work is still in progress and will be published with reproducible measurements rather than provisional numbers.

## Runtime policy

NVIDIA feature runtimes are expected to come from an authorized NVIDIA source and are checked before use. DLSS Native does not ship game-extracted runtimes, driver-internal payloads or borrowed NVIDIA Project/Application IDs.

## License and provenance

The standalone Visual Enhancer source line is derived from the MIT-licensed `Merserk/dlss5-visual-enhancer` snapshot recorded in the project provenance, then extended with the Unified Core / ABI4 architecture work.

See [LICENSE](LICENSE) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

---

<div align="center">

**Native video processing, with the timeline kept intact.**

DLSS Native is an independent project and is not affiliated with or endorsed by NVIDIA. NVIDIA, GeForce RTX and DLSS are trademarks of NVIDIA Corporation.

</div>
