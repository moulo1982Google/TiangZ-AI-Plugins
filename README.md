# TiangZ AI Plugins

TiangZ 项目的 AI 插件集合，发布当前版本的 Codex、Claude 和 Cindy 游戏后端开发能力。

## 插件目录

| 目录 | 用途 |
| --- | --- |
| `.claude-plugin/marketplace.json` | Claude Code 插件市场清单，把本仓库整体作为一个本地市场 |
| `tiangz-game-backend/` | Codex 与 Claude Code 共用的插件包，包含 `.codex-plugin/plugin.json`、`.claude-plugin/plugin.json`、MCP 配置和 0.2.0 Skill |
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

推荐按插件安装，一次拿到 Skill 和 MCP。在目标项目里把本仓库添加为本地市场并启用插件：

```text
/plugin marketplace add D:/你的路径/TiangZ-AI-Plugins
/plugin install tiangz-game-backend@tiangz-local
```

也可以直接写进项目的 `.claude/settings.json`（路径按实际检出位置填）：

```json
{
  "extraKnownMarketplaces": {
    "tiangz-local": { "source": { "source": "local", "path": "./TiangZ-AI-Plugins" } }
  },
  "enabledPlugins": { "tiangz-game-backend@tiangz-local": true }
}
```

只要 Skill、不要 MCP 时，仍可把 `claude/tiangz-game-backend/` 整个目录复制到目标项目的 `.claude/skills/tiangz-game-backend/`，打开新会话后使用 `/tiangz-game-backend`。

#### MCP 前置条件

插件里的 `tiangz-design` MCP 由 `@tiangz/developer-tools-core` 提供，本仓库不含该程序。没有全局安装时 Skill 可用但 MCP 显示不可用——换一台机器后 MCP 失效通常就是这个原因：

```bash
npm install -g @tiangz/developer-tools-core
```

`.mcp.json` 里写的是 Windows 的 `tiangz-design-mcp.cmd`；macOS/Linux 需要把命令改成 `tiangz-design-mcp`。用 `npm ls -g --depth=0` 确认是否装好。

### Cindy

使用 `tiangz-game-backend-cindy/tiangz-game-backend-0.2.0.cindy` 安装，或按照 Cindy 的本地插件开发方式加载同目录的 `ghost.json` 和 `main.js`。

## 更新约定

唯一维护源在 TiangZ 主工程的 `tools/ai-assistants/`；本仓库保存可分发产物，也是工作区安装插件的唯一来源（原先的工作区 `plugins/` 本地副本已删除）。插件清单（`.claude-plugin/`、`.codex-plugin/`、`.mcp.json`）不由构建脚本生成，改版本号时要三处一起改。修改插件内容后，应在主工程运行 `node tools/ai-assistants/build.mjs`、`node tools/ai-assistants/check.mjs` 和 `node tools/ai-assistants/build.mjs --check`，再更新本仓库产物。提交前至少检查 JSON、Cindy 包、插件 Skill 和 Git diff；不要把 TiangZ、Examples、测试临时目录或运行凭据复制进本仓库。
