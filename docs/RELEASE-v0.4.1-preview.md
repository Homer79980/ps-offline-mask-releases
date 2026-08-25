# PS Offline Mask v0.4.1 Preview

## 本版修复

- 精修预览改用 Photoshop UXP 支持的 DOM/PNG 图层，滚轮缩放、画笔绘制和快捷键直接绑定可见输入平面。
- 未知、保留、删除、抓手和缩放工具改为固定 ID 与新图形，支持 `H`、`Z`、空格平移和括号调笔刷。
- 边缘参数集中到精修工作区；柔边与硬边显式互斥，去色边只在可靠背景条件下校正 RGB。
- 文件夹批量只输出透明 PNG，不创建 JSON 或其它旁车文件。
- 自动 Trimap 保护蓝眼睛等小型封闭同色细节，并继续切除达到面积门槛的真实孔洞。

## 验证结果

- 327 项 Node 测试、JavaScript 语法检查和预览发布树审计通过。
- 43 张用户练习图完成只读算法审计：36 张自动通过，7 张需人工或精修复核。
- Photoshop 未被自动操作；真实 UXP 窗口输入、蒙版应用和批量写回仍需用户验收。

## 下载校验

- 文件：`ps-offline-mask-0.4.1-uxp-preview.zip`
- SHA-256：`4773fa252c07834eb991f172ea18a4a413f7b691a719f3a112bf5e92e982a2cf`
- 解压后在 UXP Developer Tool 中添加 `plugin/manifest.json`。

本包是开发预览包，不是签名 `.ccx`。
