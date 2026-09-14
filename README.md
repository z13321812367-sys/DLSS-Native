<p align="center">
  <a href="https://z13321812367-sys.github.io/DLSS-Native/">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="./docs/assets/hero-dark.svg">
      <source media="(prefers-color-scheme: light)" srcset="./docs/assets/hero-light.svg">
      <img alt="DLSS Native — RTX video, end to end" src="./docs/assets/hero-light.svg">
    </picture>
  </a>
</p>

<h3 align="center">Native RTX video processing for Super Resolution, Frame Generation and HDR-aware workflows.</h3>

<p align="center">
  <a href="https://z13321812367-sys.github.io/DLSS-Native/">Project site</a>
  ·
  <a href="./README.zh-CN.md">中文</a>
  ·
  <a href="./THIRD_PARTY_NOTICES.md">Third-party notices</a>
</p>

<p align="center">
  Windows · NVIDIA RTX · D3D12 · Unified ABI4
</p>

---

## What it does

DLSS Native is a Windows video-processing project that keeps source timing, GPU processing and output scheduling in one D3D12 pipeline.

It is designed to:

- upscale video with **DLSS Super Resolution**;
- generate intermediate frames with **DLSS Frame Generation**;
- preserve **HDR precision and signal semantics** through the processing chain;
- keep media timing tied to real frame PTS;
- encode, reorder, mux and verify the result as one continuous workflow.

The goal is straightforward: use RTX features without turning the video path into a chain of unnecessary CPU round trips and loosely coupled conversion steps.

## Architecture

<p align="center">
  <img src="./docs/assets/architecture.svg" alt="DLSS Native architecture" width="100%">
</p>

The runtime owns the selected adapter, D3D12 device, queue, feature sessions, optical-flow session and shared surfaces as one sequence-scoped context.

That matters in three places:

**GPU ownership.** Neural stages can exchange native GPU surfaces instead of forcing every stage through host memory.

**Video timing.** Real frame PTS and decoded frame reality drive scheduling. Average FPS does not replace source timestamps when the source already provides them.

**Output correctness.** Encoding, frame reordering, muxing and end-of-stream handling are checked at the file boundary rather than inferred from a successful GPU call.

## HDR is part of the image path

DLSS Native treats HDR as image data, not a metadata checkbox. Precision, transfer semantics and the processing domain have to remain coherent across the pipeline.

An output file carrying BT.2020 / ST2084 metadata is not, by itself, proof that the intermediate processing preserved the HDR signal correctly.

## Validated baseline

<p align="center">
  <img src="./docs/assets/validation.svg" alt="DLSS Native validation baseline" width="100%">
</p>

The integrated baseline has been validated on an **RTX 5060 Ti** with the Unified **ABI4 4.0.0** runtime and the current SR / FG product path.

Performance work on the direct **D3D12 → NVENC** path is still active. Throughput numbers will be published only when the current validation line is complete and reproducible.

## Current status

| Area | Status |
| --- | --- |
| DLSS Super Resolution | **Available in the current product path** |
| DLSS Frame Generation | **Available in the current product path** |
| Unified ABI4 / D3D12 runtime | **Current architecture** |
| HDR-aware video pipeline | **Current architecture** |
| Neural Rendering / NR | **Runtime-dependent; disabled when a trusted runtime is unavailable** |
| Direct D3D12 → NVENC | **Active performance validation** |
| Public application source / binaries | **Not published yet** |

## Runtime policy

NVIDIA feature runtimes are expected to come from an authorized NVIDIA source and are checked before use. DLSS Native does not ship game-extracted runtimes, driver-internal payloads or borrowed NVIDIA Project/Application IDs.

## License and provenance

The standalone Visual Enhancer source line is derived from the MIT-licensed `Merserk/dlss5-visual-enhancer` snapshot recorded in the project provenance, then extended with the Unified Core / ABI4 architecture work.

See [LICENSE](./LICENSE) and [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md).

---

<p align="center">
  DLSS Native is an independent project and is not affiliated with or endorsed by NVIDIA.<br>
  NVIDIA, GeForce RTX and DLSS are trademarks of NVIDIA Corporation.
</p>
