# 安装说明

## 当前版本状态

当前公开下载是 `v0.3.1-preview` UXP Developer Tool 开发预览包，不是签名后的生产 `.ccx`。最低宿主为 Photoshop 25.0。纯色背景算法已通过无宿主自动化质量门；精修输入、前景图层写回、批量宿主写回和 Windows/macOS 安装仍需真实宿主验收。

发布前在仓库根目录运行 `npm run audit:release`。生产包还必须在包含三平台 Addon 和 `native/release-integrity.json` 的组装目录中，于 macOS 运行 `npm run audit:production`；AI 变体使用 `node scripts/audit-release.js --production --manifest=plugin/manifest.ai.json`。审计会检查 canonical manifest、二进制架构、SHA-256 完整性、macOS 签名及 Gatekeeper/公证状态，失败时不要上传 Release。

## 开发预览安装

1. 安装 Photoshop 25.0 或更高版本。
2. 安装 Adobe UXP Developer Tool。
3. 从 GitHub Releases 下载 `ps-offline-mask-*-uxp-preview.zip` 并解压到本地目录。
4. 打开 UXP Developer Tool，选择 **Add Plugin**，指向解压目录中的 `manifest.json`。
5. 点击 **Load**，在 Photoshop 的插件菜单中打开“离线抠图”面板。
6. 主面板会显示当前图层/蒙版结果缩略图。点击“打开抠图精修”后可立即绘制、滚轮缩放并使用 `[`/`]`；底部输入诊断只记录宿主是否转发了这些事件，不会禁用画笔。

“离线抠图”主面板范围约为 `320×420` 到 `1600×2400`。Photoshop 插件菜单只显示“离线抠图”；精修窗口由主面板按钮打开，默认设计尺寸为 `1100×820`，最小设计尺寸为 `640×480`。UXP 是否允许用户从窗口边缘改变模态窗口尺寸仍需在真实 Photoshop 25+ 验证，当前预览包不把它作为已通过能力。单入口与独立可停靠面板无法同时成立，本版按用户确认优先保留单入口。

从 `v0.2.7` 或更早版本升级时，必须在 UXP Developer Tool 中先 Stop 并移除旧实例，再从新解压目录重新 Add Plugin。不要直接覆盖旧目录后继续使用已加载实例，否则 Photoshop 可能继续运行旧 JavaScript 和旧窗口定义。

开发预览包只处理当前文档中选中的普通、未完全锁定的像素图层；单选一个像素图层时会自动读取，手动“刷新”作为同层内容更新的兜底；多选时进入批量模式并处理全部可写入的选中像素图层。文字、智能对象、组和矢量图层需要先在 Photoshop 中栅格化。“背景”图层需要先双击转换为普通图层；完全锁定的像素图层需要先解锁。插件会直接提示或在批量中跳过这些不可写入图层。

本预览版只以 Photoshop 25+ 的直接 Imaging 写入为发布目标。仓库中的旧兼容代码不属于 v0.3.1-preview 支持声明。

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
