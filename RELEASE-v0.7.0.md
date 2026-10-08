# v0.7.0

TiangZ 六仓库套件 0.7.0 正式版中的 AI 插件（GitHub Release，不表示客户端 Marketplace 上架）。候选标签 `v0.7.0-rc1` 与其附件保持不变。

## 本仓库改动

- 插件版本 0.3.0-rc.1 → **0.3.0**（Codex/Claude 清单、Cindy `ghost.json` 与归档文件名）。
- 随包 MCP 由 Developer Core **0.16.1**（tiangz-developer-tools `v0.16.1`）重新构建，并经 TiangZ `v0.7.0` 的 `tools/ai-assistants/distribute.py` 分发。bundle 与 rc.1 相比只有握手版本字符串不同；新 SHA256 为 `2991560853ed249a56c1372e39729619fbdcb5df5e389507cdeba5ab9c4825a3`，构建锁 SHA256 `1009dab4c2701dfd132367bb4b3161fb52deae3219d3469fad3fd17a2f00e4da`。
- 技能与开发约束文件未变。

## 验证

- `distribute.py --check`：13 项、0 差异。
- 客户端实际安装与 MCP 握手验收记录仍是 0.3.0-rc.1 的结果（2026-09-27）；本次未重做。
