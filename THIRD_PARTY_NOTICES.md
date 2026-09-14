# Third-party notices — Unified Core

## ComfyUI-DLSS5-NR

Phase 4's experimental DLSS Neural Rendering backend adapts portions of the Feature-18 parameter contract and caller-shim technique from:

- Project: `lisitskyaa/ComfyUI-DLSS5-NR`
- License: MIT
- Copyright (c) 2026 ComfyUI-DLSS5-NR contributors

The adapted work includes the minimal NGX parameter-object ABI shape, Feature-18 parameter names/resource conventions, and the separate noinline caller-shim technique required by the DLSSNR snippet runtime. Unified Core does **not** launch ComfyUI and does not include its application/UI layer.

MIT License

Copyright (c) 2026 ComfyUI-DLSS5-NR contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## FFmpeg / Gyan Windows build

Phase 11 portable staging may include `ffmpeg.exe` and `ffprobe.exe` from the
Gyan FFmpeg **full** Windows build:

- Provider: `https://www.gyan.dev/ffmpeg/builds/`
- Upstream FFmpeg download page: `https://ffmpeg.org/download.html`
- Payload alias used by the release builder: `ffmpeg-release-full.7z`
- Integrity source: the provider's adjacent `.sha256` endpoint
- Provider-declared binary license for the full static build: GPLv3

The source repository does not commit those Windows executables. The release
payload preparation script downloads the archive and its publisher checksum,
verifies the archive SHA-256 before extraction, capability-checks the resulting
`ffmpeg.exe`/`ffprobe.exe`, and writes `bin/ffmpeg/payload-provenance.json` with
the archive and binary identities. The portable release manifest binds that
provenance file and the staged binary hashes.

Technical staging verification is **not** a public-distribution authorization.
A public release that conveys the GPL-covered FFmpeg binaries must separately
satisfy the applicable GPL license and Corresponding Source obligations for the
exact distributed binaries. The Phase 11 technical staging manifest therefore
keeps FFmpeg public source-compliance status explicit instead of treating a
successful codec/runtime smoke as a legal-release approval.

## NVIDIA runtime components

The source repository does **not** commit NVIDIA proprietary runtime DLLs, NGX SDK headers, models, or driver components.

Phase 11.3 private clean-Windows validation artifacts may contain exactly the official DLSS Super Resolution and Frame Generation runtime DLLs (`nvngx_dlss.dll` and `nvngx_dlssg.dll`) supplied by the release builder from an operator-provided NVIDIA DLSS SDK checkout. The Phase 11.3 builder requires explicit SDK-license acknowledgement, validates NVIDIA Authenticode signatures, records SHA-256 identities, and stages the SDK's original `LICENSE.txt` as `licenses/NVIDIA_RTX_SDK_LICENSE.txt`.

Those Phase 11.3 packages are **private validation artifacts**, not an automatic claim that all NVIDIA public-release, branding, notification, trademark, or other distribution obligations have been completed. Public release authorization remains a separate fail-closed gate.

Feature-18 Neural Rendering remains outside the official portable profile. Phase 11.3 does not download, copy, rename, substitute, or bundle `nvngx_dlssnr.dll`, and it does not substitute the separate Ray Reconstruction runtime `nvngx_dlssd.dll` for Feature-18.

NVIDIA's public NGX headers currently expose numeric Feature 18 as a reserved feature rather than a public Feature-18 parameter contract. Accordingly, the experimental Feature-18 backend keeps its ABI declarations local and does not claim that those parameter names are part of NVIDIA's public supported API.
