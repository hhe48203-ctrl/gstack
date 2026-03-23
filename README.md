# gstack

> 将 Claude Code 变成你的虚拟工程团队 —— 20 个专家角色，8 个强力工具，全部通过斜杠命令调用。

## 简介

gstack 是一套基于 Claude Code 的开源软件工厂。它将 AI 编程助手从单一的"副驾驶"升级为一个完整的虚拟工程团队：CEO 重新思考产品方向、工程经理锁定架构、设计师捕捉 AI 生成的低质量内容、评审员发现生产环境 bug、QA 负责人在真实浏览器中测试、安全官运行 OWASP + STRIDE 审计、发布工程师负责提交 PR。

**适用人群：**
- **创始人和 CEO** —— 尤其是仍然想要亲自写代码发布产品的技术型创始人
- **Claude Code 新手** —— 用结构化角色替代空白提示词
- **技术主管和资深工程师** —— 每个 PR 都有严格的评审、QA 和发布自动化

## 快速开始

1. 安装 gstack（30 秒，见下方说明）
2. 运行 `/office-hours` —— 描述你想构建的产品
3. 运行 `/plan-ceo-review` —— 对任何功能想法进行评审
4. 运行 `/review` —— 评审任何有变更的分支
5. 运行 `/qa` —— 对你的测试环境进行 QA 测试
6. 到此为止，你就能判断这个工具是否适合你

## 安装 —— 30 秒

**前置要求：** [Claude Code](https://docs.anthropic.com/en/docs/claude-code)、[Git](https://git-scm.com/)、[Bun](https://bun.sh/) v1.0+、[Node.js](https://nodejs.org/)（仅 Windows 需要）

### 第一步：安装到本机

打开 Claude Code，粘贴以下内容，Claude 会自动完成安装：

> Install gstack: run **`git clone https://github.com/hhe48203-ctrl/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup`** then add a "gstack" section to CLAUDE.md that says to use the /browse skill from gstack for all web browsing, never use mcp\_\_claude-in-chrome\_\_\* tools, and lists the available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review, /design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse, /qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro, /investigate, /document-release, /codex, /cso, /autoplan, /careful, /freeze, /guard, /unfreeze, /gstack-upgrade. Then ask the user if they also want to add gstack to the current project so teammates get it.

### 第二步：添加到项目仓库（可选，让队友也能使用）

> Add gstack to this project: run **`cp -Rf ~/.claude/skills/gstack .claude/skills/gstack && rm -rf .claude/skills/gstack/.git && cd .claude/skills/gstack && ./setup`** then add a "gstack" section to this project's CLAUDE.md that says to use the /browse skill from gstack for all web browsing, never use mcp\_\_claude-in-chrome\_\_\* tools, lists the available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review, /design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse, /qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro, /investigate, /document-release, /codex, /cso, /autoplan, /careful, /freeze, /guard, /unfreeze, /gstack-upgrade, and tells Claude that if gstack skills aren't working, run `cd .claude/skills/gstack && ./setup` to build the binary and register skills.

文件会直接提交到你的仓库（不是 submodule），`git clone` 即可使用。所有内容都在 `.claude/` 目录下，不会修改你的 PATH 或后台运行任何进程。

### 支持 Codex、Gemini CLI 或 Cursor

gstack 兼容任何支持 [SKILL.md 标准](https://github.com/anthropics/claude-code) 的 AI 代理。

安装到单个仓库：

```bash
git clone https://github.com/hhe48203-ctrl/gstack.git .agents/skills/gstack
cd .agents/skills/gstack && ./setup --host codex
```

全局安装（用户级别）：

```bash
git clone https://github.com/hhe48203-ctrl/gstack.git ~/gstack
cd ~/gstack && ./setup --host codex
```

自动检测已安装的 AI 代理：

```bash
git clone https://github.com/hhe48203-ctrl/gstack.git ~/gstack
cd ~/gstack && ./setup --host auto
```

## 工作流程：完整的冲刺循环

gstack 不是一堆零散的工具，而是一个完整的流程。技能按照冲刺的顺序运行：

**思考 → 规划 → 构建 → 评审 → 测试 → 发布 → 复盘**

每个技能的输出会流入下一个环节。`/office-hours` 生成设计文档供 `/plan-ceo-review` 阅读；`/plan-eng-review` 生成测试计划供 `/qa` 执行；`/review` 发现的 bug 由 `/ship` 验证修复。

## 核心技能一览

| 技能 | 角色 | 功能说明 |
|------|------|----------|
| `/office-hours` | **YC 办公时间** | 起点。六个关键问题重新定义你的产品方向，挑战你的假设，生成实施方案。设计文档会传递给所有下游技能。 |
| `/plan-ceo-review` | **CEO / 创始人** | 重新思考问题。四种模式：扩展、选择性扩展、保持范围、缩减。 |
| `/plan-eng-review` | **工程经理** | 锁定架构、数据流、图表、边界情况和测试方案。 |
| `/plan-design-review` | **高级设计师** | 对每个设计维度打分 0-10，说明 10 分是什么样的，然后编辑计划达到目标。AI 低质量内容检测。 |
| `/design-consultation` | **设计合伙人** | 从零开始构建完整的设计系统。研究市场、提出创新风险、生成产品原型。 |
| `/review` | **资深工程师** | 找出通过 CI 但在生产环境中会崩溃的 bug。自动修复显而易见的问题，标记完整性缺口。 |
| `/investigate` | **调试专家** | 系统化的根因调试。铁律：没有调查就没有修复。追踪数据流，测试假设，3 次失败后停止。 |
| `/design-review` | **会写代码的设计师** | 与 /plan-design-review 相同的审计，然后修复发现的问题。原子提交，前后截图对比。 |
| `/qa` | **QA 负责人** | 测试应用、发现 bug、用原子提交修复、重新验证。为每个修复自动生成回归测试。 |
| `/qa-only` | **QA 报告员** | 与 /qa 相同的方法论，但仅生成报告，不修改代码。 |
| `/cso` | **首席安全官** | OWASP Top 10 + STRIDE 威胁建模。零噪音：17 个误报排除项，8/10+ 置信度门槛，独立验证每个发现。 |
| `/ship` | **发布工程师** | 同步主分支、运行测试、审计覆盖率、推送代码、创建 PR。如果没有测试框架会自动搭建。 |
| `/land-and-deploy` | **发布工程师** | 合并 PR，等待 CI 和部署，验证生产环境健康。一条命令从"已批准"到"生产验证通过"。 |
| `/canary` | **SRE** | 部署后监控循环。监视控制台错误、性能回归和页面故障。 |
| `/benchmark` | **性能工程师** | 基线页面加载时间、Core Web Vitals 和资源大小。每个 PR 前后对比。 |
| `/document-release` | **技术文档工程师** | 更新所有项目文档以匹配刚发布的内容。自动捕捉过时的 README。 |
| `/retro` | **工程经理** | 团队感知的周回顾。按人员细分、发布连胜、测试健康趋势、成长机会。`/retro global` 跨所有项目和 AI 工具运行。 |
| `/browse` | **QA 工程师** | 真实的 Chromium 浏览器，真实的点击，真实的截图。每条命令约 100ms。 |
| `/setup-browser-cookies` | **会话管理** | 从你的真实浏览器（Chrome、Arc、Brave、Edge）导入 cookie 到无头会话。测试需要登录的页面。 |
| `/autoplan` | **评审流水线** | 一条命令，完整评审的计划。自动运行 CEO → 设计 → 工程评审。只有品味决策才需要你批准。 |

### 增强工具

| 技能 | 功能说明 |
|------|----------|
| `/codex` | **第二意见** —— 来自 OpenAI Codex CLI 的独立代码审查。三种模式：评审（通过/失败门控）、对抗性挑战、开放咨询。 |
| `/careful` | **安全护栏** —— 在执行破坏性命令前警告（rm -rf、DROP TABLE、force-push）。说"小心点"即可激活。 |
| `/freeze` | **编辑锁定** —— 限制文件编辑只能在一个目录内。防止调试时意外修改范围外的文件。 |
| `/guard` | **完整安全** —— `/careful` + `/freeze` 合一。生产环境工作时的最高安全级别。 |
| `/unfreeze` | **解锁** —— 移除 `/freeze` 的限制。 |
| `/setup-deploy` | **部署配置** —— 一次性配置 `/land-and-deploy`。自动检测平台、生产 URL 和部署命令。 |
| `/gstack-upgrade` | **自更新** —— 升级 gstack 到最新版本。检测全局/本地安装，同步两者，显示变更内容。 |

## 并行冲刺

gstack 支持多个冲刺并行运行。[Conductor](https://conductor.build) 可以同时运行多个 Claude Code 会话，每个会话在独立的工作空间中。一个在 `/office-hours`，另一个在 `/review`，第三个实现功能，第四个运行 `/qa`。冲刺结构使并行成为可能——没有流程，十个代理就是十个混乱源。

## 隐私与遥测

- **默认关闭。** 除非你明确同意，不会发送任何数据。
- **首次运行时，** gstack 会询问你是否愿意分享匿名使用数据。你可以拒绝。
- **发送的内容（如果你同意）：** 技能名称、持续时间、成功/失败、gstack 版本、操作系统。
- **永远不会发送：** 代码、文件路径、仓库名称、分支名称、提示词或任何用户生成的内容。
- **随时更改：** `gstack-config set telemetry off` 立即禁用一切。

## 故障排除

**技能没有显示？** `cd ~/.claude/skills/gstack && ./setup`

**`/browse` 失败？** `cd ~/.claude/skills/gstack && bun install && bun run build`

**安装过时？** 运行 `/gstack-upgrade` —— 或在 `~/.gstack/config.yaml` 中设置 `auto_upgrade: true`

**Windows 用户：** gstack 在 Windows 11 上通过 Git Bash 或 WSL 运行。除 Bun 外还需要 Node.js。

**Claude 说看不到技能？** 确保项目的 `CLAUDE.md` 中有 gstack 配置段。添加以下内容：

```
## gstack
Use /browse from gstack for all web browsing. Never use mcp__claude-in-chrome__* tools.
Available skills: /office-hours, /plan-ceo-review, /plan-eng-review, /plan-design-review,
/design-consultation, /review, /ship, /land-and-deploy, /canary, /benchmark, /browse,
/qa, /qa-only, /design-review, /setup-browser-cookies, /setup-deploy, /retro,
/investigate, /document-release, /codex, /cso, /autoplan, /careful, /freeze, /guard,
/unfreeze, /gstack-upgrade.
```

## 许可证

MIT 开源许可。免费使用，永远免费。
