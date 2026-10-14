# TiangZ AI Plugins 0.3.3 候选

本候选同步开发约束维护源与 Developer Core 0.16.3，帮助 AI 区分设计建议、静态检查、真实 Model 加载、数据库恢复和目标客户端运行证据。宿主仍为独立版本序列的 TiangZ 0.7.3，不创建新框架标签或 Marketplace 发布。

新增五项确定性规则：`model.export-shape`、`protocol.post-merge`、`persistence.no-change-identity`、`persistence.domain-integration`、`validation.target-execution`。Model 函数/构造函数导出合法，实例 Map/Set 字段不因导出约束被禁止；pending 与历史回执先于 no-op 成功判断。领域接线与 mailbox 选择依实际所有权和合同，不统一放开并发或写入所有 no-op。

三个 MCP 工具、现有请求/响应格式与只读权限保持兼容。Cindy 保留既有工具和建议入口。维护源来自 TiangZ `tools/ai-assistants/`，正式生成并显式分发；本仓库不手改生成技能或 MCP bundle。

验收记录：

- 13 个正式分发产物、12 个文件哈希、Cindy CRC、技能格式、源/分发字节对应通过；提取 MCP 握手 0.16.3、29 项规则、三只读工具及 Quest 建议通过。
- Cindy 0.3.3 归档 SHA256：`5559169063fe40bc4bc00de7920127e25e838fee400212530d93bae113bb1660`。
- MCP bundle SHA256：`cb7afb57e7089cfbda1fa6717e07c50031cf4ec0ad66d7e3fd9b09fed76b148b`。
- 实际 Codex 临时会话调用 MCP，独立评审确认九项提供证据后的核心判断符合规则。首次文件读取遭客户端策略拒绝；后续未放宽策略，通过主会话提供原文与真实检查结果继续评估。模型后端身份未知，独立文件读取/命令执行未验证；下一步证据清单有生命周期、仍未知、真实 DB、不同键并发及逐帧核对缺项，完整 A07 仅部分关闭。
- Claude/Cindy 安装、新会话 UI 和 Marketplace 发布本轮未执行。历史归档与标签不覆盖。

更新与复现入口见 [README.md](README.md)。候选规则不提供原本未存在于目标 checkout 的 API，用户必须核对实际宿主与依赖身份。
