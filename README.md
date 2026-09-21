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

推荐按插件安装，一次拿到 Skill 和 MCP。先把本仓库克隆到本机，再在目标项目的会话里添加市场并安装：

```bash
git clone https://github.com/moulo1982Google/TiangZ-AI-Plugins
```

```text
/plugin marketplace add <上一步的克隆路径>
/plugin install tiangz-game-backend@tiangz-local
```

市场名 `tiangz-local` 由本仓库的 `.claude-plugin/marketplace.json` 定义，不要另取。

也可以直接写进项目的 `.claude/settings.json`，跳过交互命令。`path` 相对于放这个 `.claude/` 的目录；本仓库不在该目录下时填绝对路径：

```json
{
  "extraKnownMarketplaces": {
    "tiangz-local": { "source": { "source": "local", "path": "./TiangZ-AI-Plugins" } }
  },
  "enabledPlugins": { "tiangz-game-backend@tiangz-local": true }
}
```

改完配置要新开一个会话才生效。装好后 `/tiangz-game-backend` 可用、MCP 列表里有 `tiangz-design`，缺任何一个都说明没装全。

只要 Skill、不要 MCP 时，仍可把 `claude/tiangz-game-backend/` 整个目录复制到目标项目的 `.claude/skills/tiangz-game-backend/`，打开新会话后使用 `/tiangz-game-backend`。

#### MCP 前置条件

插件里的 `tiangz-design` MCP 由 `@tiangz/developer-tools-core` 提供，本仓库不含该程序，只有它全局可用时 `.mcp.json` 才能拉起服务。换一台机器后 Skill 能用但 MCP 不可用，基本都是缺这一步。

**这个包没有发布到 npm**，只能从源码仓库装，需要 Node.js 20 以上：

```bash
git clone https://github.com/moulo1982Google/tiangz-developer-tools
cd tiangz-developer-tools
npm install
npm install -g .
```

`npm install` 会通过 `prepare` 构建出 `dist/*.cjs`，`npm install -g .` 再把命令装到全局。不要用 `npm install -g @tiangz/developer-tools-core`（registry 上没有，报 404），也不要用 `npm install -g github:moulo1982Google/tiangz-developer-tools`（不装开发依赖，`prepare` 找不到 `tsc`，构建失败）。

验证（Git Bash / macOS / Linux）：

```bash
npm ls -g --depth=0   # 应列出 @tiangz/developer-tools-core
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"smoke","version":"1"}}}' | tiangz-design-mcp
```

第二条应立即回一行 JSON，其中有 `"serverInfo":{"name":"tiangz-design"`。报命令找不到，说明 npm 全局 bin 目录不在 PATH 里。

`.mcp.json` 里写的是 Windows 的 `tiangz-design-mcp.cmd`；macOS/Linux 要把命令改成 `tiangz-design-mcp`，否则 MCP 起不来。

### Cindy

使用 `tiangz-game-backend-cindy/tiangz-game-backend-0.2.0.cindy` 安装，或按照 Cindy 的本地插件开发方式加载同目录的 `ghost.json` 和 `main.js`。

## 更新约定

唯一维护源在 TiangZ 主工程的 `tools/ai-assistants/`；本仓库保存可分发产物，也是工作区安装插件的唯一来源（原先的工作区 `plugins/` 本地副本已删除）。插件清单（`.claude-plugin/`、`.codex-plugin/`、`.mcp.json`）不由构建脚本生成，改版本号时要三处一起改。修改插件内容后，应在主工程运行 `node tools/ai-assistants/build.mjs`、`node tools/ai-assistants/check.mjs` 和 `node tools/ai-assistants/build.mjs --check`，再更新本仓库产物。提交前至少检查 JSON、Cindy 包、插件 Skill 和 Git diff；不要把 TiangZ、Examples、测试临时目录或运行凭据复制进本仓库。
