# v0.7.1

TiangZ 套件 0.7.1 中的 AI 插件（GitHub Release，不表示客户端 Marketplace 上架）。之前的标签与附件保持不变。

## 本仓库改动

- 插件版本 0.3.0 → **0.3.1**（Codex/Claude 清单、Cindy `ghost.json` 与归档文件名）。
- 随包 MCP 由 Developer Core **0.16.2**（tiangz-developer-tools `v0.16.2`，打包工具链依赖审计清零）重新构建，并经 TiangZ `v0.7.1` 的 `tools/ai-assistants/distribute.py` 分发。bundle 与 0.3.0 相比只有握手版本字符串不同；新 SHA256 `1e23a9ee2980d776cc21967e18db4ae3cbe1eafacb0cb30c0bd7fb7500bfe609`，构建锁 SHA256 `91c5f594e3caca418f27981de6e448983031c8a6e29e1392ea371aba89384c34`。
- 技能与开发约束文件未变。

## 验证

- `distribute.py --check`：13 项、0 差异。
- 客户端实际安装与 MCP 握手验收记录仍是 0.3.0-rc.1 的结果（2026-09-27）；本次未重做。
