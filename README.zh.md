# dsh-client-ui-aqua（适配 dsh 0.1.2-rc.1 优化版）

> **优化分支**：基于 [WYH66666666/DSH-Transparent-UI-Plugin](https://github.com/WYH66666666/DSH-Transparent-UI-Plugin)（原作者：**John Wu**）的源码优化发布，修复了兼容性问题，使 Aqua 玻璃拟态主题可在当前 DeepSeek Harness 版本上正常运行。

[English](README.md) | 中文

Aqua 是 DeepSeek Harness Web 界面的一款高度可定制的玻璃拟态主题：顶栏、侧边栏、输入区、状态栏与轨迹视图都会变成磨砂玻璃面板，支持视频壁纸，一键关闭即可完整还原官方界面——无需改动 DSH 本身任何源码。

## 为什么有这个分支

上游 npm 包 `dsh-client-ui-aqua@1.3.1`（2026-08-17 发布）是为 DSH `0.1.0-rc.x` 架构编写的：当时 `sessions`/`workspaces` 客户端服务由 `@deepseek-ai/dsh-client-runtime` 提供。DSH `0.1.2-rc.1`（2026-09-03）把这些服务重构进了官方 controller，上游作者本人也声明插件尚未适配新 API。在 `0.1.2-rc.1` 上直接安装上游包会报错：

- `Failed to load plugins: loader fibers failed` —— `@deepseek-ai/dsh-api-session-controller` / `@deepseek-ai/dsh-api-workspace-controller`（客户端服务重复注册）
- `keyed slot "settings.plugin.item" requires options.key`

## 与上游 1.3.1 的差异

| 文件 | 修改 |
| --- | --- |
| `lib/client.js` | `require("@deepseek-ai/dsh-client-runtime/client")` → `require("@deepseek-ai/dsh-client-store")` —— dsh 0.1.2-rc.1 内置该模块，导出与 Aqua 所用相同的 `defineStore` API，不再需要 runtime（及其冲突的服务注册） |
| `lib/client.js` | `settings.plugin.item` 注册补上 `key: "aqua"`（0.1.2-rc.1 中该槽位为 keyed 类型） |
| `package.json` | 从 `dsh.client.inject` 与 `peerDependencies` 移除 `@deepseek-ai/dsh-client-runtime`；版本号改为 `1.3.1-optimized.1` |

**请勿与本分支同时安装 runtime**：`dsh-client-runtime` 会与 0.1.2-rc.1 的官方 controller 重复注册 `sessions`/`workspaces` 服务，重新触发 `loader fibers failed` 冲突。profile 的 patch 中不应存在 `dsh-client-runtime` 的 insert 条目。

### 已知限制

插件页的"Glass theme"总开关卡片在 0.1.2-rc.1 上不会被调度显示（该 keyed 槽位只渲染 host 已注册的命名空间，而 Aqua 是纯前端插件）；**主题默认开启**，外观控制在 **设置 → 通用 → 外观** 中可调（`settings.general.item` 为 list 槽位，会渲染所有注册者）。

## 安装方式（GitHub / 本地路径）

不要用 `dsh plugin add`（它会拉取上游 npm 包并覆盖这些修复），请直接从本仓库安装：

1. 克隆或下载本仓库。
2. 将 `dsh-client-ui-aqua` 目录复制到 web profile 的 node_modules：

   ```
   %DSH_HOME%\profiles\web\node_modules\dsh-client-ui-aqua
   ```

3. 在 `%DSH_HOME%\profiles\web\cordis.patch.yml` 中注册插件行：

   ```yaml
   - insert:
       - id: ui-aqua
         name: 'dsh-client-ui-aqua'
   ```

4. 重启 `dsh web`（或桌面包装应用）。

主题立即生效，可在 **设置 → 通用 → 外观** 中开关与调整。

## 许可证

[MIT](LICENSE) —— Copyright (c) 2026 **John Wu**（原作者）。本分支在原许可下修改并再分发，保留原始版权声明。
