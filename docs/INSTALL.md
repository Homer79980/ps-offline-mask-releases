# 安装说明

## 当前版本

`v0.4.1-preview` 是 Photoshop 25.0+ 的 UXP Developer Tool 开发预览包，不是签名 `.ccx`。Windows 和 macOS 使用同一份 JavaScript 预览包；生产 CCX 仍需三平台运行时、签名、公证及真实 Photoshop 回归。

自动验证已覆盖 327 项逻辑和契约测试、43 张本地练习图的只读算法审计、语法检查与预览发布树审计。练习集有 36 张通过自动残留门，7 张需人工或精修复核。开发过程不会自动操作用户的 Photoshop；实际画笔、模态窗口、蒙版写回与文件夹选择器需由用户安装后验收。

## 安装开发预览包

1. 安装 Photoshop 25.0 或更高版本。
2. 安装 Adobe UXP Developer Tool。
3. 从公开仓库 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases) 下载 `ps-offline-mask-0.4.1-uxp-preview.zip`。
4. 对照 Release 中的 SHA-256 校验下载文件。
5. 解压 ZIP。打开 UXP Developer Tool，选择 **Add Plugin**，指向解压目录中的 `plugin/manifest.json`。
6. 点击 **Load**，再从 Photoshop 的“增效工具/插件”菜单打开“离线抠图”。菜单中只应有一个入口。

从旧版升级时，先在 UXP Developer Tool 中 Stop 并移除旧实例，再解压新包并重新 Add Plugin。不要覆盖仍在加载的旧目录，否则 Photoshop 可能继续运行旧脚本和旧窗口定义。

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

## AI 视觉模型变体

默认包完全离线且不申请网络权限。要测试视觉模型接口，先生成只允许指定 HTTPS 服务域名的清单：

```powershell
npm run prepare:manifest -- --domain=https://your-api.example.com
```

再由可信宿主注入 `globalThis.__PS_MASK_VISION_API__` 和运行时密钥提供器。不要把 API Key、Cookie、Authorization、私有端点或客户素材写入仓库或发布包。契约见 [VISION_API.md](VISION_API.md)。

## 卸载

在 UXP Developer Tool 中 Stop 并移除插件即可。插件不会修改源码目录中的图片；已应用到 Photoshop 文档的图层或蒙版应通过 Photoshop 历史记录管理。
