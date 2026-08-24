# PS Offline Mask Releases

Photoshop 抠图插件的公开下载仓库。

![0.2.0 面板预览](assets/ui-preview-0.2.png)

## 当前下载

请前往 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases) 下载最新版本：

- `ps-offline-mask-*-uxp-preview.zip`：UXP Developer Tool 开发预览包
- 生产 `.ccx`：待 C++ Hybrid 原生引擎、Windows/macOS 签名和 Photoshop 24.4+ 回归完成后发布

`v0.2.0-preview` 增加 5%–800% 自由缩放、适合窗口、本地一键抠图、可取消计算、真实形态学边缘偏移、严格图层边界校验，以及默认关闭的通用 AI 视觉模型 API 接口。

本次修复还确保面板会先绑定按钮再初始化宿主增强能力，避免 UXP 生命周期或尺寸 API 异常时出现“能看到但点不动”的面板；浮动面板支持在宿主允许范围内调整大小（约 `320×420` 到 `1600×2400`）。

源码、测试和 PRD 位于私有源码仓库，不在本仓库维护。

## 安装与使用

- [安装说明](docs/INSTALL.md)
- [使用说明](docs/USAGE.md)

## 兼容性

- Photoshop 24.4+（使用稳定版 Imaging API）
- Windows x64、macOS Intel x64、macOS Apple Silicon arm64（开发预览阶段以 UXP 宿主实测为准）
- 默认纯本地算法；AI 视觉模型 API 为可选运行时配置

## 校验

每个 Release 会附带 `SHA256SUMS.txt`。下载后建议先校验 SHA-256，再解压安装。

## 许可与反馈

当前项目尚未发布正式开源许可证。使用、再分发或集成到商业产品前，请先联系仓库维护者。问题反馈请使用本仓库的 Issues，不要提交任何 PSD、API Key、Cookie 或客户素材。
