# PS Offline Mask Releases

Photoshop 抠图插件的公开下载仓库。

![0.2 面板预览](assets/ui-preview-0.2.png)

![0.2 抠图精修窗口](assets/refine-preview-0.2.png)

## 当前下载

请前往 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases) 下载最新版本：

- `ps-offline-mask-*-uxp-preview.zip`：UXP Developer Tool 开发预览包
- 生产 `.ccx`：待 C++ Hybrid 原生引擎、Windows/macOS 签名和 Photoshop 24.4+ 回归完成后发布

`v0.2.8-preview` 继续保持 Photoshop 插件菜单只有“离线抠图”一个入口。主面板保留当前图层/蒙版结果缩略图，并通过“打开抠图精修窗口”按钮打开可缩放的大尺寸模态工作区；窗口默认 `1100×820`，最小 `640×480`。

精修画布同时兼容 Pointer 与传统鼠标输入，支持围绕鼠标位置滚轮缩放、空格加左键平移，以及未知/保留/删除三种画笔。独立 Canvas 会绘制清晰的半透明双圈和实时 `px` 标签，`[` / `]` 调整画笔直径；专用裁切层避免大笔刷在图像边缘撑大滚动范围。柔软度会真实写入 Trimap 的 UNKNOWN 过渡区域。点击“应用蒙版”时会先关闭精修窗口，再写入 Photoshop；失败后精修内容仍保留。

Photoshop 24.4–24.7.x 会自动使用“临时选区 → 用户蒙版”的兼容写入方式，避开旧宿主 `putLayerMask` 对大块黑色 Alpha 的已知缺陷。兼容流程会验证当前目标图层，并在失败时按快照恢复原选区、图层多选和原用户蒙版。Photoshop 25+ 继续使用直接 Imaging 写入。蒙版存在性无法可靠读取时会中止写入，不再把宿主查询错误误判为“没有蒙版”。

“背景”图层和完全锁定的图层不会直接进入蒙版写入。请先双击“背景”转换为普通像素图层，或先解除图层锁定；批量模式会跳过这些图层并显示具体原因，避免 Photoshop 24.x 的 `-32005` 创建失败。

插件打开、切换文档或切换单个像素图层时会自动读取。即使切换发生在重算、应用或批处理期间，也会在当前操作安全结束后重查新图层；旧图层缩略图、Trimap、结果和精修会话不会残留。“刷新”保留为同一图层像素变化时的手动兜底。

批量处理仍固定使用本地算法并严格串行。非像素、背景和完全锁定图层会跳过；单层失败会恢复该层原蒙版后继续，如果连恢复也失败则立即停止，避免继续修改后续图层。文档、图层选择或边界变化也会停止后续写入。

从旧版本升级时，请在 UXP Developer Tool 中 Stop 并移除旧实例，重新解压当前 Release 包，然后 Add/Load 新目录中的 `manifest.json`。不要覆盖正在加载的旧目录；否则 Photoshop 可能继续运行旧 JavaScript 和旧窗口定义。本版 133 项自动化测试、语法检查、发布审计和多轴代码复审已通过。按用户要求，本轮没有自动操作 Photoshop；Photoshop 24.4 中的画笔绘制、单层应用和批量流程由用户安装后真机复测。

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
