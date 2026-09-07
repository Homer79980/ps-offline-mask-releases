# CCX 安装排查

## 当前结论

`ps-offline-mask-0.1.0.ccx` 已由 Adobe UXP Developer Tools 2.2.1 生成，并通过 Windows Unified Plugin Installer Agent 8.5.0.13 安装验证。安装命令返回 `Installation Successful` 和退出码 `0`，插件列表显示 `PS Offline Mask 0.1.0 / Enabled`。

当前 CCX SHA-256：`0E38D53AE66DE0EE17A5D5FF50A67F3285AA050B8AA38FC850B57705C8D63498`。

包内共有 54 个运行文件，全部与私有源码仓的 `plugin/` 发布树一致。压缩包中只有一个根目录 `manifest.json`，没有重复路径、反斜杠路径、外层 `plugin/` 包装目录或第二份 AI 清单。

Adobe 文档说明 `.ccx` 底层是 ZIP，但建议由 UXP Developer Tool 生成，而不是手工压缩：

- [Adobe Photoshop UXP 打包说明](https://developer.adobe.com/photoshop/uxp/guides/distribution/packaging-your-plugin/)
- [Adobe UXP 插件安装说明](https://developer.adobe.com/premiere-pro/uxp/plugins/distribution/install/)

## 错误代码 -1

2026-09-07 早期上传的同名包由 Windows 压缩命令生成，内部大部分路径使用 `\`，而 Adobe UDT 生成的 CCX 使用 `/`。该包虽然可能被 UPIA 部分解压，Creative Cloud 仍会显示“无法安装插件，错误代码 -1”。公开 Release 中的文件已经替换。

1. 删除本机旧下载，重新下载 `ps-offline-mask-0.1.0.ccx`。
2. 校验 SHA-256，必须与本页顶部的新值一致。
3. 在 Creative Cloud Desktop 的“插件 > 管理插件”检查 `PS Offline Mask`。若旧包已留下 0.1.0，先卸载它。
4. 双击新 CCX，选择 **Install locally/本地安装** 并确认。
5. 重启 Photoshop，从“增效工具/插件”菜单打开“离线抠图”。

## 开发者加载备用路径

如果校验值正确但 Creative Cloud 仍无法安装：

1. 安装 Adobe UXP Developer Tool。
2. 将 CCX 复制一份改名为 ZIP 并解压。
3. 使用 **Add Plugin** 选择解压根目录的 `manifest.json`。
4. 点击 **Load**；不要同时加载旧目录或安装版。

不要通过修改插件 ID、添加第二份 manifest、第三方破解补丁或来历不明的 sideloader 处理 `-1`。需要继续定位时，点击 Creative Cloud 错误提示里的“详细信息”，并附上对应日志与实际 SHA-256。
