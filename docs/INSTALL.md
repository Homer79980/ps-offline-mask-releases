# 安装说明

## 当前版本状态

当前公开下载是 UXP Developer Tool 开发预览包，不是签名后的生产 `.ccx`。它用于验证面板、Trimap、Photoshop 画布临时蒙版预览和本地算法流程。生产 `.ccx` 需要后续接入 C++ Hybrid 原生引擎、完成 Windows/macOS 签名，并在 Photoshop 24.4+ 上验收。

发布前在仓库根目录运行 `npm run audit:release`。生产包还必须在包含三平台 Addon 和 `native/release-integrity.json` 的组装目录中，于 macOS 运行 `npm run audit:production`；AI 变体使用 `node scripts/audit-release.js --production --manifest=plugin/manifest.ai.json`。审计会检查 canonical manifest、二进制架构、SHA-256 完整性、macOS 签名及 Gatekeeper/公证状态，失败时不要上传 Release。

## 开发预览安装

1. 安装 Photoshop 24.4 或更高版本。
2. 安装 Adobe UXP Developer Tool。
3. 从 GitHub Releases 下载 `ps-offline-mask-*-uxp-preview.zip` 并解压到本地目录。
4. 打开 UXP Developer Tool，选择 **Add Plugin**，指向解压目录中的 `manifest.json`。
5. 点击 **Load**，在 Photoshop 的插件菜单中打开“离线抠图”面板。
6. 主面板会显示当前图层/蒙版结果缩略图。点击“打开抠图精修工作区”进入可拉伸的独立精修面板；只有自动打开失败时才需要从插件菜单单独打开“抠图精修”。

“离线抠图”主面板范围约为 `320×420` 到 `1600×2400`；“抠图精修”面板范围约为 `480×420` 到 `3000×3000`，推荐浮动尺寸为 `1100×820`。两个面板都由 Photoshop 管理，可以停靠或拖出并调整大小。Photoshop 会自动把每个原生 UXP 面板列入插件菜单，因此菜单中会同时出现“离线抠图”和“抠图精修”；后者是主面板自动打开失败时的备用入口，UXP 没有在保留独立可拉伸面板时隐藏它的官方配置。

从 `v0.2.4` 升级时必须在 UXP Developer Tool 中先 Stop 并移除旧实例，再从新解压目录重新 Add Plugin。`v0.2.5` 修复了旧包的 `create method is not defined for plugin` 初始化错误；不要直接覆盖旧目录后继续使用已加载实例。

开发预览包只处理当前文档中选中的像素图层；单选一个像素图层时会自动读取，手动“刷新”作为同层内容更新的兜底；多选时进入批量模式并处理全部选中像素图层。文字、智能对象、组和矢量图层需要先在 Photoshop 中栅格化。

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
