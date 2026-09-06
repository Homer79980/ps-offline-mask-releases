# PS Offline Mask Releases

Photoshop 离线抠图插件的公开下载与使用文档。源码在私有仓库维护，本仓库只发布安装包、校验值和面向用户的说明。

![离线抠图主面板](assets/ui-preview-v0.4.png)

![离线抠图精修工作区](assets/refine-preview-v0.4.png)

## 当前版本

[`v0.1.0`](https://github.com/Homer79980/ps-offline-mask-releases/releases/tag/v0.1.0) 面向 Photoshop 25.0+：

- 安装包：`ps-offline-mask-0.1.0.ccx`
- SHA-256：`FE461228BD1BF004D6A1C482611D7BA5CCE639E8722B06186D696A6F59DFF2A0`
- [安装说明](docs/INSTALL.md)
- [操作说明](docs/USAGE.md)
- [本版变更](docs/RELEASE-v0.1.0.md)

## 安装

先下载 CCX 并尝试双击安装。当前安装包尚未经过 Adobe Marketplace 签名；如果 Creative Cloud 拒绝未签名包，把 CCX 复制并改名为 ZIP，解压后在 UXP Developer Tool 中选择 **Add Plugin**，指向根目录的 `manifest.json`，再点击 **Load**。

升级时先 Stop 并移除旧实例，不要覆盖正在加载的旧目录。重启 Photoshop 后，从“增效工具/插件”菜单打开“离线抠图”。

## 使用

1. 在 Photoshop 中选择普通、未完全锁定的像素图层。
2. 点击“一键抠图”，先在插件预览中检查透明结果。
3. 需要时进入“精修”，切换白、黑或自定义背景检查边缘，并调整收缩、柔边、硬边或去色边。
4. 重新计算后点击“应用”。插件保留源图，并以新图层或可撤销蒙版写入结果。

图层批量和文件夹批量固定使用本地算法。AI 视觉服务为可选功能；只有主动配置服务、授权外发且单图本地结果需要辅助时才会调用。

## 兼容性与边界

- Photoshop 25.0+
- Windows x64、macOS Intel x64、macOS Apple Silicon arm64，以实际 UXP 宿主为准
- 最适合透明图、棋盘格、纯色/近纯色背景、游戏和 UI 素材
- 发丝、玻璃、烟雾、复杂场景背景及主体与背景高度同色时，可能需要精修或专业 Alpha 模型

## 反馈

请使用本仓库 Issues。不要提交 PSD、API Key、Cookie、Authorization 头或客户素材。
