# 安装说明

## 开发预览包

当前 Release 提供的是 UXP Developer Tool 开发预览包，不是签名后的生产 `.ccx`。

1. 安装 Photoshop 24.4 或更高版本。
2. 安装 Adobe UXP Developer Tool。
3. 从本仓库的 [Releases](https://github.com/Homer79980/ps-offline-mask-releases/releases) 下载 `ps-offline-mask-*-uxp-preview.zip`。
4. 解压后，在 UXP Developer Tool 中选择 **Add Plugin**，指向解压目录中的 `manifest.json`。
5. 点击 **Load**，回到 Photoshop，从插件菜单打开“离线抠图”。

面板支持在 Photoshop 中拖拽浮动窗口边缘调整大小，宿主允许的范围为约 `320×420` 到 `1600×2400`。如果旧实例仍保持旧尺寸，请在 UXP Developer Tool 中 Stop 后重新 Load 一次。

开发预览包只支持像素图层。文字、智能对象、组和矢量图层请先栅格化，或直接选择像素图层。

## 安装前校验

Release 附带 `SHA256SUMS.txt`。Windows PowerShell 可执行：

```powershell
Get-FileHash .\ps-offline-mask-*-uxp-preview.zip -Algorithm SHA256
```

将输出的哈希与 `SHA256SUMS.txt` 对比后再解压。

## 生产版说明

签名 `.ccx` 尚未发布。生产版需要包含签名的 C++ Hybrid 原生引擎，并分别完成 Windows x64、macOS Intel x64、macOS Apple Silicon arm64 的安装和运行验收。开发预览不应作为生产版本分发。
