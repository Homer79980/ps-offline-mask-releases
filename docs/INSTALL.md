# 安装说明

## 当前版本状态

当前公开下载是 UXP Developer Tool 开发预览包，不是签名后的生产 `.ccx`。它用于验证单入口面板、Trimap 精修窗口、插件内蒙版预览和本地算法流程。生产 `.ccx` 需要后续接入 C++ Hybrid 原生引擎、完成 Windows/macOS 签名，并在 Photoshop 24.4+ 上验收。

发布前在仓库根目录运行 `npm run audit:release`。生产包还必须在包含三平台 Addon 和 `native/release-integrity.json` 的组装目录中，于 macOS 运行 `npm run audit:production`；AI 变体使用 `node scripts/audit-release.js --production --manifest=plugin/manifest.ai.json`。审计会检查 canonical manifest、二进制架构、SHA-256 完整性、macOS 签名及 Gatekeeper/公证状态，失败时不要上传 Release。

## 开发预览安装

1. 安装 Photoshop 24.4 或更高版本。
2. 安装 Adobe UXP Developer Tool。
3. 从 GitHub Releases 下载 `ps-offline-mask-*-uxp-preview.zip` 并解压到本地目录。
4. 打开 UXP Developer Tool，选择 **Add Plugin**，指向解压目录中的 `manifest.json`。
5. 点击 **Load**，在 Photoshop 的插件菜单中打开“离线抠图”面板。
6. 主面板会显示当前图层/蒙版结果缩略图。点击面板内的“打开抠图精修窗口”进入大尺寸精修工作区。

“离线抠图”主面板范围约为 `320×420` 到 `1600×2400`。Photoshop 插件菜单只显示“离线抠图”；精修窗口由主面板按钮打开，默认设计尺寸为 `1100×820`，最小 `640×480`，并可从窗口边缘缩放。单入口与独立可停靠面板无法同时成立，本版按用户确认优先保留单入口。

从 `v0.2.6` 或更早版本升级时，必须在 UXP Developer Tool 中先 Stop 并移除旧实例，再从新解压目录重新 Add Plugin。不要直接覆盖旧目录后继续使用已加载实例，否则 Photoshop 可能继续运行旧 JavaScript 和旧窗口定义。

开发预览包只处理当前文档中选中的像素图层；单选一个像素图层时会自动读取，手动“刷新”作为同层内容更新的兜底；多选时进入批量模式并处理全部选中像素图层。文字、智能对象、组和矢量图层需要先在 Photoshop 中栅格化。

Photoshop 24.4–24.7.x 会自动使用“临时选区 → 用户蒙版”的兼容写入方式，以避开旧宿主对大块黑色 Alpha 的已知写入缺陷；Photoshop 25+ 使用直接 Imaging 写入。兼容流程会恢复原选区和原图层多选，失败时原子回滚原用户蒙版。

## 从源码运行

源码仓库为私有仓库。维护者可以克隆源码后执行：

```powershell
npm install
npm test
npm run check
```

随后在 UXP Developer Tool 中加载 `plugin/manifest.json`。

## AI 视觉模型（可选）

默认模式完全离线。签入仓库的 `plugin/manifest.json` 不申请网络权限。要启用 AI 模式，先运行 `npm run prepare:manifest -- --domain=https://your-api.example.com` 生成带明确域名白名单的 `plugin/manifest.ai.json`，再在 UXP Developer Tool 中加载该 manifest。AI 模式只有在宿主运行时注入 `globalThis.__PS_MASK_VISION_API__` 后才会出现，并且用户必须明确确认图层数据外发。远程端点必须使用 HTTPS；不要把 API Key、Cookie 或私有端点写入源码或 Release 包。

## 卸载

在 UXP Developer Tool 中停止并移除开发预览插件即可。它不会修改原始像素；已应用的图层蒙版仍可通过 Photoshop 历史记录或图层蒙版编辑撤销。
