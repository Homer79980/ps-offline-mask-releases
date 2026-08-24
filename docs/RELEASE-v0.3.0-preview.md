# v0.3.0-preview 发布说明

这是面向 Photoshop 25.0+ 和 UXP Developer Tool 的开发预览包，不是签名 `.ccx`。

## 本版变化

- 纯色/近纯色背景的单图和批量流程共用同一个确定性 matte 引擎。
- 全图背景分类支持封闭在主体内部的背景孔洞，不再只处理与画布外边缘连通的背景。
- UNKNOWN 过渡区输出连续 Alpha，保留抗锯齿软边；相近前景/背景无法可靠区分时失败关闭。
- Photoshop 插件菜单保持一个“离线抠图”入口；精修窗口从主面板打开。
- 精修输入改为立即响应；可信输入探针只记录画笔、滚轮、快捷键和坐标映射诊断，不再吞掉首次事件或锁死画笔。
- Vision API 保持默认关闭。v1 支持 Alpha；v2 可选返回校正前景 RGB。包内没有 API 端点、凭据或 AI 模型。
- 本地模型只保留 fail-closed adapter；许可证和运行时尚未通过门禁的模型不会进入安装包。

## 自动化证据

- Node 测试：203/203 通过。
- 1024×1024 封闭孔洞 fixture：中位 174 ms，IoU 1.0，孔洞最大 Alpha 0。
- 1024×1024 抗锯齿 fixture：中位 84 ms，边缘 MAE 0.001310，连续 Alpha 保留率 100%。
- 额外难例：旋转内孔泄漏率 0、3.25 px 细斜线 IoU 0.980、软阴影 MAE 0.032、JPEG 式量化噪声边缘 MAE 0.024。
- 语法检查、凭据/符号链接扫描、单入口/Photoshop 25 manifest 审计和严格预览 staging 通过。
- 生产审计保持失败：缺少 Windows x64、macOS x64、macOS arm64 原生 Addon、生产 manifest 和完整性登记。

## 仍需真机验证

按用户要求，发布过程没有自动操作 Photoshop。以下能力不能仅凭 mock 或浏览器测试宣称完成：

- Photoshop 25+ 中精修输入探针、滚轮缩放、空格平移和三种 Trimap 画笔。
- 单层蒙版应用和可撤销回滚。
- 多选图层批量写回与失败恢复。
- Vision API v2 校正前景的新图层写入。
- Windows 和 macOS 三架构安装、签名、Gatekeeper 与 notarization。

如果某项输入诊断一直缺失，请保留底部完整状态并反馈；它不会阻止其它已由宿主转发的操作。若菜单仍出现第二个“抠图精修”入口，请 Stop/Remove 旧插件并从新的解压目录重新 Add Plugin。
