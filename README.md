# TiangZ AI Plugins

TiangZ 项目的 AI 插件集合，发布当前版本的 Codex、Claude 和 Cindy 游戏后端开发能力。

## 插件目录

| 目录 | 用途 |
| --- | --- |
| `tiangz-game-backend/` | Codex 插件包，包含 `.codex-plugin/plugin.json`、MCP 配置和 0.2.0 Skill |
| `claude/tiangz-game-backend/` | Claude Code 使用的 0.2.0 Skill |
| `tiangz-game-backend-cindy/` | Cindy 插件源码和 `tiangz-game-backend-0.2.0.cindy` 分发包 |

插件中的规则用于辅助开发 TiangZ、TiangZ-DBProxy 和 TiangZ-Examples，包括模块边界、协议生成、Timer、持久化、热更和测试证据。具体项目代码和版本化开发约束仍以对应工程仓库为准，插件不会替代工程文档。

## 使用方式

### Codex

在 Codex 插件管理器中安装或链接 `tiangz-game-backend/`。插件清单位于：

```text
tiangz-game-backend/.codex-plugin/plugin.json
```

### Claude Code

将 `claude/tiangz-game-backend/` 整个目录复制到目标项目的 `.claude/skills/tiangz-game-backend/`，打开新会话后使用 `/tiangz-game-backend`。

### Cindy

使用 `tiangz-game-backend-cindy/tiangz-game-backend-0.2.0.cindy` 安装，或按照 Cindy 的本地插件开发方式加载同目录的 `ghost.json` 和 `main.js`。

## 更新约定

唯一维护源在 TiangZ 主工程的 `tools/ai-assistants/`；本仓库保存可分发产物。修改插件内容后，应在主工程运行 `node tools/ai-assistants/build.mjs`、`node tools/ai-assistants/check.mjs` 和 `node tools/ai-assistants/build.mjs --check`，再更新本仓库产物。提交前至少检查 JSON、Cindy 包、插件 Skill 和 Git diff；不要把 TiangZ、Examples、测试临时目录或运行凭据复制进本仓库。
