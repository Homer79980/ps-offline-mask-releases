# PS Offline Mask Releases

Photoshop 抠图插件的公开下载仓库。

![0.2 面板预览](assets/ui-preview-0.2.png)

## 当前下载

请前往 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases) 下载最新版本：

- `ps-offline-mask-*-uxp-preview.zip`：UXP Developer Tool 开发预览包
- 生产 `.ccx`：待 C++ Hybrid 原生引擎、Windows/macOS 签名和 Photoshop 24.4+ 回归完成后发布

`v0.2.2-preview` 修复本轮 Photoshop 实测反馈：插件打开或切换单个像素图层后会自动读取，“刷新”仅作为同层内容更新的兜底；预览图支持滚轮缩放以及空格 + 左键拖动平移；未知、保留和删除工具的圆形画笔在 Photoshop 24.4 的 UXP 宿主中保持可见并可涂抹；批量抠图可直接处理尚未创建用户蒙版的普通像素图层。

批量处理固定使用本地算法并严格串行。非像素图层会跳过，单层失败会恢复该层原蒙版并继续，文档、图层选择或边界变化会立即停止后续写入。

批量失败时面板会显示图层名和具体错误原因，不再只显示“失败 N 层”。无原始蒙版的图层会先记录可逆空快照；若应用中途失败，插件会删除临时蒙版并继续处理下一层。

从旧版本升级时，请在 UXP Developer Tool 中 Stop 旧实例，重新解压当前 Release 包，然后 Add/Load 新目录中的 `manifest.json`。不要覆盖正在加载的旧目录。

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
