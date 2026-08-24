# v0.2.5-preview

## 本轮修复

- 修复 Photoshop 24.4 / UXP 7 加载时的 `create method is not defined for plugin` 错误；补齐插件级 `create()` 生命周期后，主面板按钮可以正常绑定。
- 在“离线抠图”主面板恢复只读缩略图：读取图层后显示源图，生成结果后显示蒙版缩略图。
- 缩略图使用独立 `quick-preview-*` 节点，不复制精修面板的画布 ID，避免滚轮、画笔和输入监听绑定到错误面板。
- 保留“打开抠图精修工作区”作为默认入口，并保留可停靠、浮动、自由拉伸的独立原生精修面板。
- 补充双菜单说明：Photoshop 会自动显示每个原生 UXP Panel，第二项是自动打开失败时的备用入口，无法在保留独立可拉伸面板时隐藏。

## 验证

- 自动化测试：`108/108` 通过。
- JavaScript 语法检查：通过。
- 开发预览包安全审计：通过。
- `520×960` 和最小宽度 `320px` 主面板静态布局验收：通过。
- ZIP 与私有源码仓 `plugin/` 的 25 个运行文件逐文件 SHA-256 一致。
- 两路独立代码审查通过：标准问题 0，需求偏差 0。
- 遵守用户要求，本轮没有由自动化流程操作 Photoshop；安装后请由用户完成真实宿主复测。

## 升级与测试

1. 在 UXP Developer Tool 中 Stop 并移除旧插件。
2. 下载并解压 `ps-offline-mask-0.2.5-uxp-preview.zip` 到新目录，不要覆盖旧目录。
3. Add Plugin 并选择新目录中的 `manifest.json`，随后点击 Load。
4. 从 Photoshop 插件菜单打开“离线抠图”，确认标题显示 `v0.2.5 preview`，且底部不再出现 `create method is not defined for plugin`。
5. 选择像素图层，确认主面板自动读取并显示缩略图；计算后确认缩略图切换为蒙版结果。
6. 点击主面板的“打开抠图精修工作区”，依次测试窗口拉伸、滚轮缩放、空格拖动、三种画笔、`[` / `]`、柔软度、重新计算、应用和取消。

## 校验值

```text
93f8c1b23c94448eaab3c92baef2c432c21b7e239fa84b027d514c93ce55fbd2  ps-offline-mask-0.2.5-uxp-preview.zip
```
