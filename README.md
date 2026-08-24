# PS Offline Mask Releases

Photoshop 抠图插件的公开下载仓库。

![0.2 面板预览](assets/ui-preview-0.2.png)

## 当前下载

请前往 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases) 下载最新版本：

- `ps-offline-mask-*-uxp-preview.zip`：UXP Developer Tool 开发预览包
- 生产 `.ccx`：待 C++ Hybrid 原生引擎、Windows/macOS 签名和 Photoshop 24.4+ 回归完成后发布

`v0.2.3-preview` 针对 Photoshop 24.4 / UXP 7.0 重做预览输入层：同时兼容现代与传统鼠标事件，滚轮可直接缩放，拖动与松键具有 document 级兜底，移出面板或失焦不会留下粘住的笔画。

Trimap 工具现在显示半透明双圈画笔；`[` / `]` 调整大小，“柔软度”会真实写入 UNKNOWN 过渡外环和保留/删除确定内核。新增“专注精修”模式，可让预览占满当前面板；需要独立摆放时可把 Photoshop 面板拖成浮动窗口。

批量处理仍固定使用本地算法并严格串行。非像素图层会跳过，单层失败会恢复该层原蒙版并继续，文档、图层选择或边界变化会立即停止后续写入。

从旧版本升级时，请在 UXP Developer Tool 中 Stop 旧实例，重新解压当前 Release 包，然后 Add/Load 新目录中的 `manifest.json`。不要覆盖正在加载的旧目录。本版自动化测试与发布审计已通过；Photoshop 画笔、滚轮和平移仍需由用户安装后真机复测。

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
