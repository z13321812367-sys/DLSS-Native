<div align="center">

<img src="docs/assets/dlss-native-mark.svg" width="92" alt="DLSS Native logo">

# DLSS Native

### 面向视频处理的原生 RTX 管线，覆盖超分辨率、帧生成与 HDR 工作流。

**Windows · NVIDIA RTX · D3D12 · Unified ABI4**

[项目主页](https://z13321812367-sys.github.io/DLSS-Native/) · [English](README.md) · [第三方声明](THIRD_PARTY_NOTICES.md)

</div>

---

## 项目简介

DLSS Native 是一个面向 Windows 的 RTX 视频处理项目。它把源视频时间轴、GPU 处理和最终输出调度放在同一条 D3D12 管线中，而不是把每个阶段拆成相互独立的转换步骤。

当前产品路径由 Unified ABI4 runtime 统一管理 DLSS Super Resolution、Frame Generation、光流 guidance 和 HDR 处理。

```text
媒体
  → 解码 + 源时间轴
  → 共享 D3D12 / RTX Context
  → Super Resolution
  → 必要时进行 HDR 处理
  → Frame Generation
  → 确定性输出时间线
  → 编码 / 封装 / 验证
```

## 核心特点

### 原生 GPU 所有权

Runtime 统一管理所选 GPU、D3D12 device、queue、feature session、光流 session 和共享 surface。神经处理阶段之间可以直接传递 GPU surface，避免把每一步都变成 CPU 往返。

### 正确的视频时间轴

调度以真实 frame PTS 和实际解码结果为准。源视频已经提供时间戳时，不会再用平均帧率去替代媒体时间。

### HDR 作为图像信号处理

HDR 路径不仅关心元数据，也保留精度、transfer semantics 和处理域的一致性。输出带有 BT.2020 / ST2084 标记，并不自动等于 HDR 处理正确。

### 输出端验证

编码、帧重排、mux 和 EOS drain 都属于视频管线的一部分。最终正确性在文件边界进行验证，而不是只看 GPU 调用是否成功。

## 当前状态

| 功能 | 状态 |
| --- | --- |
| DLSS Super Resolution | **当前产品路径可用** |
| DLSS Frame Generation | **当前产品路径可用** |
| Unified ABI4 / D3D12 runtime | **当前架构** |
| HDR-aware 视频管线 | **当前架构** |
| Neural Rendering / NR | **依赖可信 runtime；不可用时默认关闭** |
| Direct D3D12 → NVENC | **正在进行性能验证** |
| 公开源码 / 二进制 | **尚未发布** |

当前集成产品基线已在 RTX 5060 Ti 上完成验证。性能方向仍在继续推进，后续会直接公布可复现的测试结果，不提前放出临时数字。

## Runtime 原则

NVIDIA feature runtime 需要来自合法、授权的 NVIDIA 来源，并在使用前进行检查。DLSS Native 不分发从游戏提取的 runtime、驱动内部 payload，也不复用第三方 NVIDIA Project/Application ID。

## License 与来源

Standalone Visual Enhancer 源线来自项目 provenance 中记录的 MIT-licensed `Merserk/dlss5-visual-enhancer` snapshot，随后加入 Unified Core / ABI4 架构工作。

具体见 [LICENSE](LICENSE) 与 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

---

<div align="center">

**原生视频处理，时间轴保持完整。**

DLSS Native 是独立项目，与 NVIDIA 无隶属或官方背书关系。NVIDIA、GeForce RTX 与 DLSS 是 NVIDIA Corporation 的商标。

</div>
