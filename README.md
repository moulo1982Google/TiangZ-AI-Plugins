> 本轮发布：套件 TiangZ 0.7.0 正式版（标签 `v0.7.0`），插件 0.3.0，随包 MCP 由 Developer Core 0.16.1 重新分发。历史 RC 标签、测试资格和制品保持原身份；本次说明见 [RELEASE-v0.7.0.md](RELEASE-v0.7.0.md)。

# TiangZ AI Plugins

TiangZ 0.7.0 的 AI 插件版本为 **0.3.0**，保持独立版本序列。本次通过套件标签 v0.7.0 发布 GitHub 正式版；不表示客户端 Marketplace 上架。

| 目录 | 内容 |
| --- | --- |
| `tiangz-game-backend/` | Codex / Claude Code 插件：技能、开发约束与随包的只读 MCP |
| `claude/tiangz-game-backend/` | 可单独安装的 Claude Skill |
| `tiangz-game-backend-cindy/` | Cindy 源码和 `tiangz-game-backend-0.3.0.cindy` |
| `.claude-plugin/marketplace.json` | Claude Code 本地市场 `tiangz-local` |
| `distribution-manifest.json` | 12 个生成文件的 SHA256 与插件/Core 版本 |

技能覆盖 TiangZ、DBProxy、Examples 的模块边界、协议、Timer、持久化、热更和验收证据。规则和设计建议不能替代当前检出的代码、生成锁及正式检查。

## Codex 与 Claude Code

两个客户端都需要 Node.js 20 以上。MCP 已随插件打包，不再要求全局安装 Developer Tools，也无需修改 Windows/macOS/Linux 命令名。

Codex 使用插件管理器添加本地 `tiangz-game-backend/`；团队或个人市场应指向这一完整目录。Codex 清单引用根 `.mcp.json`，由客户端将 `cwd: "."` 解析为安装目录，再启动 `node mcp/tiangz-design-mcp.cjs`。

Claude Code 在目标项目会话中添加本仓库的克隆路径并安装：

```text
/plugin marketplace add <TiangZ-AI-Plugins 克隆路径>
/plugin install tiangz-game-backend@tiangz-local
```

新会话中应识别 `tiangz-game-backend:tiangz-game-backend` 技能与 `tiangz_design` MCP。Claude 清单引用 `.mcp.claude.json`，覆盖同名服务，使用 `${CLAUDE_PLUGIN_ROOT}` 的实际安装路径。不要把这一变量直接复制到 Codex 的 MCP 参数。

只需要技能时，可将 `claude/tiangz-game-backend/` 复制到目标项目 `.claude/skills/tiangz-game-backend/`，该方式不安装 MCP。

MCP 来自 Developer Core **0.16.1**，提供 `list_design_rules`、`infer_system_archetype`、`recommend_system_design` 三个只读工具。`mcp/build-info.json` 记录实际 bundle、构建锁及依赖许可证哈希，握手版本必须与该清单一致。分发时保留 `mcp/LICENSE`、`mcp/NOTICE` 与第三方 `NOTICES.txt`。

路径依据：[Codex MCP 加载器](https://github.com/openai/codex/blob/main/codex-rs/codex-mcp/src/plugin_config.rs)、[Claude 插件清单规范](https://code.claude.com/docs/en/plugins-reference)。以实际客户端验收结果确认支持范围。

## Cindy

安装 `tiangz-game-backend-cindy/tiangz-game-backend-0.3.0.cindy`。首次使用可选择客户端的本地模式；登录页尚无数据所有者时不能启动插件沙箱。

插件包含四个只读工具、六类设计建议。环境工具明确返回“未探测”，不会把旧版本号当成当前工作区版本，也不执行编译、安装、数据库操作或服务启动。

## 实际验收

2026-09-27 使用隔离配置验证（对象是 0.3.0-rc.1 与 MCP 0.16.1-rc.2；0.3.0 只换版本号与 MCP 构建身份，客户端验收未重做）：

- Codex CLI 0.158.0-alpha.2 安装并加载候选技能；客户端真实 MCP 调用返回设计规则和 Quest 建议。
- Claude Code 2.1.275 校验清单，通过新控制会话发现技能；MCP 连接成功，服务报告 0.16.1-rc.2，三工具均标为只读。
- Cindy 0.1.93 对实际归档 inspect/install/reload 成功，客户端沙箱状态为 running；安装文件与归档哈希另行核对。
- 实际归档的 12 个文件哈希、技能校验、四个 Cindy 工具与六类建议通过。Cindy 工具行为是归档测试结果；未冒充通过客户端模型对话或 Forge 发布审核。上述客户端验收均未发起模型推理。

原始客户端日志、版本、包身份及失败原因在联合宿主的 `temp/v0.7-ai-client-acceptance/` 与 `temp/v0.7-ai-rc1-artifact-identity.json`。原 `.cmd` 全局依赖和硬编码 0.13.0 握手已修复；保留最初反例，不以成功重跑替换失败事实。生产用户配置未安装或更新本候选。

## 更新与复现

技能/Cindy 的维护源在 TiangZ `tools/ai-assistants/`；MCP 的维护源在 Developer Tools。客户端清单及 MCP 启动配置在本仓库维护。不可手改生成技能或 bundle。

在选定的 TiangZ 检出中运行：

```text
node tools/ai-assistants/build.mjs
node tools/ai-assistants/check.mjs
node tools/ai-assistants/build.mjs --check
python tools/ai-assistants/distribute.py --repository <本分发仓库>
python tools/ai-assistants/distribute.py --repository <本分发仓库> --check
```

分发默认使用该宿主正式安装的 Developer Core，并在复制前核对版本和哈希；联合开发可显式传 `--developer-tools <构建后的 Developer Tools>`。脚本不会全局安装插件。Cindy、Codex、Claude 三份插件清单版本必须一致。旧归档保留作历史证据，安装时选择这里注明的候选文件；不要将测试 profile、凭据或运行日志复制进分发包。
