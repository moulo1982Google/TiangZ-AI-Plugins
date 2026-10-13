# v0.7.3

TiangZ 0.7.3 对应的 AI 插件（GitHub Release，不表示客户端 Marketplace 上架）。之前的标签与附件保持不变。套件的其他仓库（TiangZ 0.7.3、DBProxy v0.7.1、Developer Tools v0.16.2）不在本次发布内；随包 MCP 不变（Developer Core 0.16.2，bundle SHA256 与 0.3.1 相同）。

## 本仓库改动

- 插件版本 0.3.1 → **0.3.2**（Codex/Claude 清单、Cindy `ghost.json` 与归档文件名）。
- 技能新增规则：
  - 在 handler 里等待 DBProxy 或其他 Host 的 Scene，不要与登记、路由、状态这类控制请求共用 ordered mailbox；用 unordered 加按键协程锁，控制请求保持同步，await 之后重新检查共享记录（GouDao 上线风暴实测：ordered 把目录限制在每秒约 250 个请求，改后上线 50 秒降到 15 秒、错误 1312 降到 0）。
  - 登记续期的超时要覆盖最坏排队时间，并给与登记有效期相同的宽限，一次续期慢不应拒绝业务。
  - 0.7.2：Watcher `process.lifecycle.restart.policy: "persistent"`；游戏 Native crate 的 `deno_core` 必须与宿主一致（0.7.2 起为 0.412）；0.7.3 起启动器核对宿主 `--version`，改包版本后要重建宿主。
  - 0.7.3：`.tiangz-dev.lock` 残留只有确认持有进程已退出才自动清除，不要手删，`.tiangz-dev.lock.takeover` 不入库。
  - 验证：长稳内存趋势要在预热之后比较，不要首尾比；共用机器上同事工作时段的负载波动与个别跳帧不直接算回归，需与空闲时段对照并写进报告。
- 开发约束参考文档（`development-contract.md`）与 0.3.1 相同。

## 验证

- TiangZ 分支 `chore/ai-plugin-0.3.2`：`node tools/ai-assistants/build.mjs --check` 与 `check.mjs`（可移植技能、4 个 Cindy 工具、6 类建议）通过。
- `tools/ai-assistants/distribute.py`：13 项产物、复查 0 差异；重打的 0.3.1 Cindy 归档与仓库里已提交的逐字节相同，说明本次归档可复现。
- 客户端实际安装与 MCP 握手验收仍是 0.3.0-rc.1 的结果（2026-09-27），本次只补充技能文字，未重做。
