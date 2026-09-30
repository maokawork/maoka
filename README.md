# dsh-client-ui-aqua · DeepSeek Harness 透明界面插件

> 基于 [WYH66666666/DSH-Transparent-UI-Plugin](https://github.com/WYH66666666/DSH-Transparent-UI-Plugin) 原作者源码优化适配而来，感谢原作者的出色工作。

针对 **DeepSeek Harness 官方桌面端（0.2.0-rc.2）** 适配与增强的 Aqua 玻璃透明 UI 插件：

- 全界面 100% 透明（对话区 / 侧边栏 / 输入框 / 弹窗 / 账号菜单），拒绝半透明
- 原版 Aqua 玻璃设置完整保留：**云母效果 / 兼容模式 / 流体 / 壁纸 / 流体颜色 / 深度 / 模糊 / 磨砂 / 背景亮度 / 粒子鲸鱼 / 小生物 / 网格 / 聚光灯 / 按压效果 / 壁纸与视频效果**
- 原版设置组件已接入官方设置页：**账号（左下角猫佧）→ 设置 → 通用设置**（滚动到底部）
- 修复官方 0.2.0 不兼容点：`IconCheckOutline16 → IconCheckOutlineRegular` 图标适配、store 契约兼容、右侧面板毛玻璃与占位处理

## 安装

1. 下载 `dsh-client-ui-aqua-1.3.1.tgz`
2. 打开 DeepSeek Harness（若未安装 CLI 会自动处理）：
   ```
   dsh plugin add dsh-client-ui-aqua-1.3.1.tgz
   ```
   或指定桌面配置：
   ```
   dsh plugin --profile desktop add dsh-client-ui-aqua-1.3.1.tgz
   ```
3. 重启 DeepSeek Harness，完成

## 设置

- 点侧边栏底部账号（猫佧）→ **设置 → 通用设置**，滚动到底部即是完整的 Aqua 玻璃设置。
- 所有设置实时生效并自动保存（localStorage `dsh.ui-aqua.*`）。

## 兼容性

- 适配版本：DeepSeek Harness 0.2.0-rc.2（官方桌面端）
- 依赖：`@deepseek-ai/dsh-client-store`（0.2.0 已内置）

## 卸载

```
dsh plugin remove dsh-client-ui-aqua
```

> 最后更新：2026-10-01（适配 DeepSeek Harness 桌面端 0.2.0-rc.2）
