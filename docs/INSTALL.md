# 安装说明（当前 v0.1.0）

## 当前版本

`v0.1.0` 是 Photoshop 25.0+ 的 UXP 插件安装包。当前 `.ccx` 由 Adobe UXP Developer Tools 2.2.1 生成，已通过 Windows Unified Plugin Installer Agent 8.5.0.13 安装验证；尚未经过 Adobe Marketplace 审核和 macOS 安装回归。当前精修提供结果预览、边缘参数、计算、应用和导航。

自动验证已通过 595 项逻辑和契约测试、语法检查与发布树审计，并完成 43 张本地练习图的只读算法审计。CCX 包含 54 个运行文件、一个根目录 `manifest.json` 和一个 Photoshop 面板入口；没有重复路径、反斜杠路径或外层包装目录。审计中仍有高残留样本被明确标记为需要视觉 API，未被伪装成“本地算法通过”。

## 推荐安装：双击 CCX

1. 安装 Photoshop 25.0 或更高版本，并确认 Creative Cloud Desktop 可以正常打开。
2. 从公开仓库 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases/tag/v0.1.0) 下载 `ps-offline-mask-0.1.0.ccx`。
3. 对照 Release 中的 SHA-256 校验下载文件。当前正确值为 `0E38D53AE66DE0EE17A5D5FF50A67F3285AA050B8AA38FC850B57705C8D63498`。
4. 双击 CCX；Creative Cloud 提示插件未经 Marketplace 验证时，选择 **Install locally/本地安装**，再确认安装。
5. 重启 Photoshop，从“增效工具/插件”菜单打开“离线抠图”。菜单中只应有一个入口。

从旧版升级时，先在 Creative Cloud 的“插件 > 管理插件”中卸载旧版本；如果还使用 UXP Developer Tool 加载过开发版，也要先 Stop 并移除旧实例。不要同时保留安装版和开发加载版，否则 Photoshop 可能继续运行旧脚本和旧窗口定义。

## `.ccx` 双击失败（错误代码 -1）

2026-09-07 早期上传的同名包使用了 Windows 反斜杠 ZIP 路径，可能触发此错误；该文件已经替换。先删除旧下载并重新下载，确认 SHA-256 是上面的新值。Creative Cloud 即使弹出 `-1` 也可能已经留下同版本插件，因此请在“插件 > 管理插件”中检查并卸载旧的 `PS Offline Mask`，然后再次双击新包。

如果新包仍然报错，可使用 Adobe 官方开发者加载作为备用路径：

1. 安装 Adobe UXP Developer Tool。
2. 将 `.ccx` 复制一份并改名为 `.zip`，解压到一个新的目录。
3. 在 UXP Developer Tool 中选择 **Add Plugin**，指向解压目录中的 `manifest.json`（不是外层目录）。
4. 点击 **Load**，确认 Photoshop 菜单只出现一个“离线抠图”入口。

这种方式不会绕过 Photoshop 的授权或安全机制，也不会修改用户的 Photoshop 文档。若需进一步定位，可点击 Creative Cloud 错误提示中的“详细信息”，或使用 Adobe Unified Plugin Installer Agent 的 `/list all` 检查插件是否其实已经安装。

## 兼容范围

- 支持普通、未完全锁定的像素图层。
- 支持 RGB 8/16/32-bit 图层读取；算法输入在本地归一化为 8-bit。
- “背景”图层需先双击转换为普通图层。
- 文字、组、智能对象和矢量图层需先在 Photoshop 中自行栅格化。
- 本地单图处理上限为 16,777,216 像素。
- 文件夹批量源格式为 PNG、JPG、JPEG、WebP，输出固定为透明 PNG。

## 从源码运行

维护者在私有源码仓库执行：

```powershell
npm install
npm test
npm run check
npm run audit:practice
npm run audit:release
```

随后在 UXP Developer Tool 中加载 `plugin/manifest.json`。

## AI 视觉模型（同一安装包，可选）

`ps-offline-mask-0.1.0.ccx` 只有一个根目录 `manifest.json` 和一个 Photoshop 入口。默认仍使用本地算法；未配置 API Key 时不会上传图片，也不会自动调用网络。无需生成或加载第二个 manifest。

由可信宿主注入（或由你的内部启动器提供）：

```js
globalThis.__PS_MASK_VISION_API__ = {
  provider: "gemini",
  model: "gemini-2.5-flash",
  apiKeyProvider: async () => obtainShortLivedGeminiKey()
};
```

然后在同一插件的“AI 设置”分页中填写任意 HTTPS OpenAI-compatible `/v1` 网关、模型 ID 和 API Key，点击“保存配置”。回到主页后不再选择引擎或逐图勾选；一键抠图会先离线计算，复杂场景自动接管已配置的视觉模型。模型能力在首次请求验证；不要把 API Key、Cookie、Authorization、私有端点或客户素材写入仓库或发布包。契约见 [VISION_API.md](VISION_API.md)。

## 使用边界

当前预览包包含自动化测试；精修窗口只调整边缘参数，点击“计算”后生成蒙版，最后由“应用”写回 Photoshop。预览画布提供抓手/缩放导航；复杂图片可在 AI 设置页配置视觉模型后自动接管。网络请求仅在用户主动配置并触发模型拉取或复杂图 AI 接管时发生。

在 UXP Developer Tool 中 Stop 并移除插件即可。插件不会修改源码目录中的图片；已应用到 Photoshop 文档的图层或蒙版应通过 Photoshop 历史记录管理。

## 面板内配置视觉模型

配置完成后，在“AI 设置”分页填写任意 OpenAI-compatible HTTPS `/v1` 网关、模型 ID 和 API Key，然后点击“保存配置”。模型名不写死，保存阶段不按名称拦截；首次 AI 请求会验证图片输入和结构化分割 JSON/Alpha 契约。只生成图片或文本的模型会在请求后给出明确提示并回退到本地候选。API Key 默认只保存在当前插件会话；只有主动勾选“记住”且宿主提供系统安全存储时才持久保存，应用后输入框会自动清空，不会写入源码、manifest、Photoshop 文档或普通配置文件。

保存后直接回到主页点击“一键抠图”；没有额外的引擎选择或逐图确认。首次复杂场景计算会完成真实鉴权和响应验证；服务不可用时保留本地候选并阻止应用。
