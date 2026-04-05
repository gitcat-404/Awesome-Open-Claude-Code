# Awesome Open Claude Code (Leak Edition) 
![演示图](first.png)
> 📦 围绕 2026 年 3 月 31 日 Claude Code npm sourcemap 泄露事件，整理的 GitHub 仓库全景索引。  
> 涵盖：原始存档、可运行修复版、Python/Rust 重写、架构分析、文档化、派生工具等各类型项目。

---

## ⚠️ 免责声明 / Disclaimer

- **所有原始源码归 Anthropic PBC 所有**，受版权保护。本列表仅用于学术研究与存档记录目的。
- Anthropic 已对多个直接镜像仓库发出 [DMCA 下架请求](https://github.com/github/dmca/blob/master/2026/03/2026-03-31-anthropic.md)，部分链接可能已失效。
- **切勿下载、编译或运行来源不明的"泄露版"代码**，已有恶意分子借此传播 Vidar 信息窃取木马等恶意软件。
- 本列表不对任何仓库的法律状态、安全性或可用性作出保证。

---

## 📌 事件背景

2026 年 3 月 31 日，安全研究员 **Chaofan Shou（@Fried_rice）** 发现 Anthropic 在发布 `@anthropic-ai/claude-code` npm 包（v2.1.88）时，意外将一个 **59.8 MB 的 JavaScript sourcemap 文件**（`.map`）打包进去，该文件包含了 **约 513,000 行未混淆 TypeScript 源码**，横跨 1,906 个文件。根源是工程师忘记在 `.npmignore` 中排除调试产物（Bun 默认生成 sourcemap）。

泄露代码随即被大量开发者下载、镜像、分析，数小时内引发了 GitHub 历史上增长最快的仓库诞生（claw-code，2 小时 5 万 Star）。Anthropic 随后紧急发出 DMCA 下架通知并撤回含 sourcemap 的 npm 包版本。工程师 Boris Cherny 确认为纯人为操作失误，非工具 Bug。

---

## 目录

- [📦 原始源码存档](#-原始源码存档)
- [🔧 可运行修复版（构建系统重建）](#-可运行修复版构建系统重建)
- [🔬 架构分析与学习解读](#-架构分析与学习解读)
- [📖 源码文档化项目](#-源码文档化项目)
- [🔄 重写与移植](#-重写与移植)
- [🛠️ 派生工具与扩展](#-派生工具与扩展)
- [📚 深度阅读 Awesome 子列表](#-深度阅读-awesome-子列表)
- [⚖️ DMCA 与法律动态](#-dmca-与法律动态)
- [🔑 泄露源码关键发现速览](#-泄露源码关键发现速览)

---

## 📦 原始源码存档

> 以归档和研究为目的的泄露 TypeScript 源码镜像。注意：多数已遭 DMCA 下架，访问前请自行判断法律风险。

### [yasasbanukaofficial/claude-code](https://github.com/yasasbanukaofficial/claude-code)

🌟 **最早发布的完整存档之一，附详尽的内部系统分解说明文档。**

泄露当天即发布，包含从 KAIROS、autoDream、ULTRAPLAN 到 Buddy 宠物系统的完整解读。发现者署名为 Chaofan Shou，作者声明未主动泄露，仅以教育存档目的归集了公开渠道的信息。

**特色：** 关键模块逐一说明 | Undercover Mode 深度解读 | 早期社区参考标杆

---

### [codeaashu/claude-code](https://github.com/codeaashu/claude-code)

🌟 **附加 MCP Server 插件，可交互式探索整个源码树。**

原始未修改的泄露源码保存在 `backup` 分支，主分支额外内置了一个 MCP Server，让 Claude Code、Claude Desktop、VS Code Copilot、Cursor 等任意 MCP 客户端可以直接"问"这份源码。

**特色：** `claude mcp add claude-code-explorer` 一键接入；支持"BashTool 是如何工作的"等自然语言查询

---

### [tanbiralam/claude-code](https://github.com/tanbiralam/claude-code)

**结构最清晰的架构文档存档。**

README 按模块系统梳理了 40+ 工具、权限系统（Default / Plan / BypassPermissions / Auto）、Bun 编译期 feature flag（PROACTIVE / KAIROS / BRIDGE_MODE / DAEMON / VOICE_MODE 等），以及 QueryEngine.ts（约 46K 行）、Tool.ts（约 29K 行）等核心大文件。

---

### [nirholas/claude-code](https://github.com/nirholas/claude-code)

**同样内置 MCP Server，原始源码保留在 `backup` 分支。**

README 详细列举了 Bun `bun:bundle` 的 dead code elimination 机制、MDM 设置与 keychain prefetch 并行预热、动态 import() 懒加载 OpenTelemetry 与 gRPC、sub-agent 通过 AgentTool 生成等高质量架构注记。

---

### [soufianebouaddis/claude-code](https://github.com/soufianebouaddis/claude-code)

**以问答 FAQ 格式组织的教育性存档，可读性极强。**

包含 BUDDY / KAIROS / Undercover Mode / Capybara/Fennel/Tengu 内部代号完整 FAQ，附 Chaofan Shou 原始推文（4.5M+ 浏览量）与多媒体报道索引。README 末尾附有"如果你是 Anthropic 法务，请开 issue"的幽默声明。

---

### [chauncygu/collection-claude-code-source-code](https://github.com/chauncygu/collection-claude-code-source-code)

🌟 **多合一集合站：原始源码 + 注释版 + nano-claude-code + 中英文分析时间线。**

分三个子目录：
1. `original-source-code/`：原始 1,884 个 `.ts/.tsx` 文件的完整快照（`src.zip`，约 9.5 MB）
2. `claude-code-source-code/`：经注释的研究版本，适合逐模块学习
3. `nano-claude-code/`：约 5,000 行 Python 极简重写

另附从泄露当天到数日后的**中英文分析文章完整时间线**（视频、技术博客、媒体报道一网打尽）。

---

### [sanbuphy/claude-code-source-code](https://github.com/sanbuphy/claude-code-source-code)

**中文社区研究向存档，独家披露了更多未公开发现。**

注重从 Agent 架构学习视角整理，包括：确认了 **Numbat** 内部代号（此前未见于其他存档）、Opus 4.7 / Sonnet 4.8 在研线索、每小时轮询 `/api/claude_code/settings` 的远程控制机制与 6+ 个 kill switch、GrowthBook flags 可在无用户感知的情况下修改任意用户行为、17 个未发布工具。

---

### [Ahmad-progr/claude-leaked-files](https://github.com/Ahmad-progr/claude-leaked-files)

**供应链安全研究视角的镜像。**

系统整理了 `src/` 目录结构（commands/ 约 50 个、tools/ 约 40 个、components/ 约 140 个），从 build artifact 泄露、软件供应链风险角度提供了分析框架。

---

### [Njengah/claude-code-source-code-leak](https://github.com/Njengah/claude-code-source-code-leak)

以"供应链泄露研究"为定位的大学生存档，完整保留了权限系统、IDE Bridge、feature flag 的说明。

---

### [zackautocracy/claude-code](https://github.com/zackautocracy/claude-code)

干净的源码镜像，结构说明简洁，适合快速检索源码文件树。

---

### [DonutShinobu/claude-code-fork](https://github.com/DonutShinobu/claude-code-fork)

源码存档 fork，保留了完整的 `src/` 目录结构，含权限系统与 Bun feature flag 的说明文档。

---

### [GitHpriyanshu23/Claude-code-leaks](https://github.com/GitHpriyanshu23/Claude-code-leaks)

含 MCP Server 配置示例（`CLAUDE_CODE_SRC_ROOT` 环境变量配置），适合跟随 `codeaashu/claude-code` 的 MCP 接入方式研究源码。

---

### [starkdcc/claude-code-original-src](https://github.com/starkdcc/claude-code-original-src)

被多篇安全分析文章（Straiker、Zscaler 等）引用的"权威存档"之一，定位纯研究目的的干净镜像。

---

### [OrcaWhisper/Claude-Code](https://github.com/OrcaWhisper/Claude-Code)

从 npm sourcemap 重建的源码存档，GitHub topic `claude-leak` 标签，更新于泄露当天（2026-03-31），是最早的镜像之一。

---

### [777genius/claude-code-source-code](https://github.com/777genius/claude-code-source-code)

简洁的源码存档仓库，基本结构说明。

---

### [leaked-claude-code/leaked-claude-code](https://github.com/leaked-claude-code/leaked-claude-code)

🚨 **声称重建了完整构建系统并"解锁"了企业级特性（存在较高安全风险，需极度谨慎）。**

声称修复了所有编译错误，提供 Windows 原生可执行文件，宣称去除限制、解锁 Enterprise 特性和 Jailbreak 模式，描述夸张。**切勿轻易运行，高度可疑。**

---

## 🔧 可运行修复版（构建系统重建）

> 泄露的原始 TypeScript 源码**无法直接运行**——缺少 `bun:bundle` polyfill、多个原生 napi 包（modifiers-napi、audio-capture-napi 等）及构建配置。以下仓库修复了这些阻塞点，使源码可以真正跑起来。

### [claude-code-best/claude-code](https://github.com/claude-code-best/claude-code)

🌟🌟 **中文社区最完整、维护最活跃的可运行修复版，堪称"扛旗项目"。**

由国内开发者维护，以 Opus 持续驱动迭代（作者自述已花费超 1000 美元 Claude 费用），几乎每隔数小时就有新提交。

**特色：**
- TypeScript 类型错误全修复，直接 `bun i && bun run dev` 启动
- OpenAI / GLM 接口兼容（`/login` 后可切换平台）
- Chrome Computer Use 支持
- 构建采用 code splitting 多文件打包，产物输出到 `dist/`（入口 `dist/cli.js` + 约 450 个 chunk），Bun 和 Node 都可启动，可发布到私有 npm 源
- 支持通过环境变量开启 `FEATURE_BUDDY=1`、`FEATURE_FORK_SUBAGENT=1` 等隐藏功能
- 在线文档（Mintlify）：[ccb.agent-aura.top](https://ccb.agent-aura.top) | DeepWiki：[deepwiki.com/claude-code-best/claude-code](https://deepwiki.com/claude-code-best/claude-code)

---

### [teze/claude-code-best](https://github.com/teze/claude-code-best)

`claude-code-best/claude-code` 的早期独立 fork，完整列出所有 **30 个 feature flag** 及大量**未公开斜杠命令**（`/ctx_viz`、`/good-claude`、`/bughunter`、`/teleport`、`/ant-trace`、`/agents-platform` 等），详细说明 polyfill 实现方式与构建产物结构。

---

### [NanmiCoder/claude-code-haha](https://github.com/NanmiCoder/claude-code-haha)

🌟 **中文社区用户量最大的可运行修复版之一（3.5k Star，4.2k Fork）。**

修复了原始源码中多个阻塞启动的关键问题（`modifiers-napi` 缺失导致 `isModifierPressed()` 抛异常、`handleEnter` 中断、`onSubmit` 永不执行等），使完整的 Ink TUI 交互界面可以在本地工作。

**特色：**
- 支持接入任意 Anthropic 兼容 API（MiniMax、OpenRouter、DeepSeek 等）
- 提供 `.env.example` 快速配置
- 支持 TUI 交互 / 无头单次问答 / 管道输入三种模式
- Computer Use（macOS，需辅助功能权限）
- Windows 支持（需 Git Bash 或直接 `bun` 调用）
- 活跃的中文 issue 社区

---

### [NanmiCoder/cc-haha](https://github.com/NanmiCoder/cc-haha)

`claude-code-haha` 的迁移版/备份仓库，内容完全相同，用于在原仓库遭受 DMCA 压力时提供稳定访问。

---

### [beita6969/claude-code](https://github.com/beita6969/claude-code)

**从零重建构建系统的研究型修复版，附 Feature Flag 使用指南。**

重建了完整的 `build.ts`（基于 Bun bundler），修复了所有缺失组件：
- `bun install && bun src/main.tsx` 直接运行
- `bun build src/main.tsx --outdir=dist --target=bun` 编译为单文件（~20 MB）
- 支持无 TTY 的无头打印模式（`--output-format text/json`）
- 附完整的 Feature Flag 详细说明与遥测禁用方法

---

### [xorespesp/claude-code](https://github.com/xorespesp/claude-code)

**结构最完整的本地可运行版本之一，完整列出了 87 个斜杠命令。**

`bun install && bun run dev` 快速启动，README 详细列出了：commands/（87 个）、services/（API/MCP/OAuth/Datadog）、components/（约 406 个 UI 组件）、coordinator/（多 Agent 编排）、bridge/（IDE 双向通信）、remote/（远程会话 teleportation）、memdir/（5 层持久化内存）、voice/（语音 STT，未发布）、buddy/（Gacha ASCII 精灵）、assistant/（KAIROS daemon，未发布）。

---

### [fazxes/Claude-code](https://github.com/fazxes/Claude-code)

**"三步可运行"的极简修复版，专注于构建流程完整性。**

构建脚本将 4,500+ 个模块打包为单文件 `dist/cli.js`（~21 MB），支持标准 OAuth 认证，提供完整遥测禁用说明，默认关闭所有 feature flag。

```bash
git clone https://github.com/fazxes/Claude-code && cd Claude-code
bun install && bun run build && bun dist/cli.js
```

---

## 🔬 架构分析与学习解读

### [Kuberwastaken/claurst](https://github.com/Kuberwastaken/claurst)

🌟 **技术分解最详尽的 README 文档，兼有 Rust 重写实现。**

此仓库在提供 Rust 移植版本的同时，输出了整个泄露事件中最有深度的技术分析文档：

**BUDDY 系统完整解码：** 物种由基于 `userId` 哈希的 Mulberry32 PRNG（盐值 `'friend-2026-401'`）确定，同一用户永远得到同一物种；物种名通过 `String.fromCharCode()` 数组混淆隐藏，共 18 种，含 Common / Uncommon / Rare / Legendary 四档；1% Shiny 概率，Shiny Legendary Nebulynx 概率 0.01%；ASCII 精灵 5 行高 × 12 字符宽，多帧动画；代码显示 2026 年 4—7 月为预告窗口，5 月正式上线。

**未公开 feature flag 完整列表（含日期）：** `redact-thinking-2026-02-12`（思维链内容脱敏）、`afk-mode-2026-01-31`（挂机模式）、`task-budgets-2026-03-13`（任务预算管理）、`advisor-tool-2026-03-01`（顾问工具，未发布）、`fast-mode-2026-02-01`（Penguin 快速模式）、`context-1m-2025-08-07`（100 万 Token 上下文）、`token-efficient-tools-2026-03-28` 等。

**未公开 Anthropic 模型代号：** Claude "Capybara"（新模型家族，已有 v2）、Claude "Tengu"（Claude Code 自身代号）、Claude "Fennel/Fennec"（Opus 变体）。

---

## 📖 源码文档化项目

### [codeaashu/claude-code-info](https://github.com/codeaashu/claude-code-info)

**文件级别的源码自动文档化项目。**

为 `claude/src` 下的每一个文件生成：文件摘要、导出项列表、依赖提示、语法高亮源码预览。配套社区文档站：[claude-code-info.vercel.app](https://claude-code-info.vercel.app)，是浏览源码结构的可视化入口。

---

## 🔄 重写与移植

> 以"干净室重写（Clean-Room Rewrite）"为法律策略，不直接包含 Anthropic 原始代码，从架构层面重新实现。

### [instructkr/claw-code](https://github.com/instructkr/claw-code)

🌟🌟 **GitHub 历史上增长最快的仓库，24 小时内突破 10 万 Star。**

由韩国开发者 **Sigrid Jin（@sigridjineth）** 于泄露当日凌晨 4 时、在感受到 DMCA 法律压力后，连夜使用 OpenAI 的 `oh-my-codex`（OmX）工具将架构从 TypeScript 重写为 Python，随后又启动 Rust 移植分支（`dev/rust`）。

- 两小时内 5 万 Star，是平台记录最快的 Star 增速之一
- 完整重新实现了 Agent harness 核心架构（工具系统、QueryEngine、多 Agent 编排、记忆管理）
- Python 层（27.1%）负责 Agent 编排；Rust 层（72.9%）处理性能关键路径
- 支持多 LLM Provider（Claude、OpenAI、本地模型），不绑定 Anthropic
- Jin 本人曾被《华尔街日报》报道为全球最活跃的 Claude Code 用户之一（年消耗 250 亿 Token）

---

### [ultraworkers/claw-code](https://github.com/ultraworkers/claw-code)

claw-code 在所有权迁移期间的**临时维护镜像**，功能与 `instructkr/claw-code` 完全一致。

---

### [ultraworkers/claw-code-parity](https://github.com/ultraworkers/claw-code-parity)

claw-code **Rust 移植分支的专项工作仓库**，包含 6 个 Rust crate 的 workspace 结构，在主仓库迁移期间承担 Rust 端进度跟踪。

---

### [SafeRL-Lab/nano-claude-code](https://github.com/SafeRL-Lab/nano-claude-code)

🌟 **最实用的 Python 极简重写，当前约 5,000 行，可立即投入使用。**

与 claw-code 侧重"架构映射"不同，nano-claude-code 是一个**真正可用的编程助手**。仅用 28 小时从 ~900 行初版迭代到 v3.0（~5,000 行），支持 20+ 闭源模型 + 本地 Ollama 任意模型，`pip install` 一键部署。

v3.0 新增：多 Agent 编排包、记忆包、技能包（内置 `/commit`、`/review`）、AI 记忆搜索、git worktree 隔离、Agent 类型定义，101 个测试。

---

## 🛠️ 派生工具与扩展

### [Enderfga/openclaw-claude-code](https://github.com/Enderfga/openclaw-claude-code)

🌟 **OpenClaw 插件，将 Claude Code 封装为统一 ISession 接口的可编程无头编程引擎。**

通过统一接口同时驱动 Claude Code、OpenAI Codex、Google Gemini 三种后端，27 个 MCP 工具 + 统一代理路由（Anthropic ↔ OpenAI 格式互转），`Council` 模式支持多 Agent 并行协作（git worktree 隔离），内置嵌入式 HTTP Server。

---

### [moazbuilds/claudeclaw](https://github.com/moazbuilds/claudeclaw)

**仿 KAIROS 架构的轻量级后台守护进程，将 Claude Code 变成"永不入睡的个人助手"。**

定时心跳检查（可配置安静时段）、Telegram + Discord 集成、语音命令转录、Web Dashboard 可视化任务状态与历史，向导式 Setup 一键配置。

---

### [Alishahryar1/free-claude-code](https://github.com/Alishahryar1/free-claude-code)

**探索"免费无限额使用 Claude Code"可行性的实验性项目。**

尝试在终端、VS Code 扩展和 Discord 上实现类似 OpenClaw 的无限额访问体验，适合研究鉴权机制与限额实现的开发者参考。

---

## 📚 深度阅读 Awesome 子列表

### [nblintao/awesome-claude-code-postleak-insights](https://github.com/nblintao/awesome-claude-code-postleak-insights)

🌟 **专门收集泄露事件后高质量分析文章的 Awesome 子列表，持续维护中。**

不包含泄露代码镜像，专注收录帮助读者理解 Claude Code 架构、安全模型、工作流设计和扩展接口的最有价值文章，包括：

- BUDDY / KAIROS / ULTRAPLAN / Undercover Mode 完整技术拆解
- WebFetchTool 深度剖析：域名白名单、服务端黑名单（5 分钟缓存）、重定向沙箱、Haiku 摘要、版权守卫（1173 行实现）
- 三层上下文压缩流水线详解：MicroCompact → AutoCompact → Full Compact
- 四阶段 AutoDream 记忆整合流程
- 两次泄露事件时间线对比（2025-02 与 2026-03）
- Hacker News 精华讨论摘录（含基于正则的情感检测、API 请求抗蒸馏防御等争议性设计）

---

## ⚖️ DMCA 与法律动态

### [github/dmca — Anthropic 下架请求（2026-03-31）](https://github.com/github/dmca/blob/master/2026/03/2026-03-31-anthropic.md)

Anthropic 向 GitHub 发出的官方 DMCA 下架请求存档，据报道覆盖了约 8,100 个仓库（部分系误发）。这是判断"直接转储"与"干净室重写"之间法律边界的核心参考文件。

**关键未解法律问题：**
- Anthropic 若主张"AI 生成的重写作品侵权"，可能反噬自身在训练数据版权案中的辩护逻辑（Gergely Orosz 详细分析）
- Clean-Room Rewrite 在本次事件中的版权保护效力尚无定论
- IPFS 等去中心化存储平台上的内容是否受 DMCA 管辖，同样是未解之问

---

## 🔑 泄露源码关键发现速览

| 模块/特性 | 位置 | 说明 |
|-----------|------|------|
| **KAIROS** 主动 Agent | `src/assistant/` | 后台 24/7，无需用户触发，每隔数秒 `<tick>` 心跳，15 秒 blocking budget |
| **autoDream** 记忆整合 | `src/services/autoDream/` | 空闲时作为子 Agent 运行，Orient→Gather→Consolidate→Prune |
| **ULTRAPLAN** 深度规划 | `src/services/ultraplan/` | 外包给远端 Opus 4.6 会话，最长 30 分钟 |
| **Undercover** 模式 | `src/utils/undercover.ts` | ~90 行，阻止内部代号外泄，无法关闭，自动去除 AI 署名 |
| **Buddy** 伴侣系统 | `src/buddy/` | Tamagotchi ASCII 宠物，18 种，Mulberry32 PRNG 确定物种，1% Shiny 概率 |
| **Feature Flags** | `bun:bundle` 编译期 | 30 个 flag，外部版本全部剥离（polyfill 为 false） |
| 未公开模型代号 | 多处字符串引用 | Capybara（v2 新家族）、Tengu（Claude Code 代号）、Numbat、Fennel/Fennec |
| 未公开斜杠命令 | `src/commands/` | `/ctx_viz`、`/teleport`、`/ant-trace`、`/agents-platform`、`/bughunter` 等 |
| 远程 Kill Switch | `src/services/` | 每小时轮询 `/api/claude_code/settings`，6+ 个危险操作拦截对话框 |
| 抗蒸馏防御 | API 请求层 | placeholder 值（`cch=ed1b0`）由 Bun 原生 HTTP 栈（Zig 实现）在传输前覆写 |
| Voice Mode | `src/voice/` | 流式语音转文字，完整实现，仅被 feature flag 门控屏蔽 |
| 5 层持久化记忆 | `src/memdir/` | `MEMORY.md` 驱动的多层记忆架构 |

---

## 📖 延伸阅读

| 标题 | 来源 | 语言 |
|------|------|------|
| [The Claude Code Source Leak: 512,000 Lines, a Missing .npmignore...](https://layer5.io/blog/engineering/the-claude-code-source-leak-512000-lines-a-missing-npmignore-and-the-fastest-growing-repo-in-github-history/) | Layer5 | 英文 |
| [Diving into Claude Code's Source Code Leak](https://read.engineerscodex.com/p/diving-into-claude-codes-source-code) | Engineers Codex | 英文 |
| [Claude Code Source Leak: With Great Agency Comes Great Responsibility](https://www.straiker.ai/blog/claude-code-source-leak-with-great-agency-comes-great-responsibility) | Straiker AI | 英文 |
| [Leaked Claude Code source spawns fastest growing GitHub repo](https://cybernews.com/tech/claude-code-leak-spawns-fastest-github-repo/) | CyberNews | 英文 |
| [Claude Code leak used to push infostealer malware](https://www.bleepingcomputer.com/news/security/claude-code-leak-used-to-push-infostealer-malware-on-github/) | BleepingComputer | 英文 |
| [Anthropic Claude Code Leak — ThreatLabz](https://www.zscaler.com/blogs/security-research/anthropic-claude-code-leak) | Zscaler | 英文 |
| [claw-code vs Claude Code: What's Actually Different?](https://wavespeed.ai/blog/posts/claw-code-vs-claude-code/) | WaveSpeed AI | 英文 |
| [Claude Code Source Code "Rebranded" Amid Wild Web Cloning](https://eu.36kr.com/en/p/3747613304193796) | 36kr | 中英双语 |
| [In the wake of the leak, 5 actions enterprise security leaders should take](https://venturebeat.com/security/claude-code-512000-line-source-leak-attack-paths-audit-security-leaders) | VentureBeat | 英文 |

---

## 贡献

欢迎提 PR 补充遗漏的仓库或修正失效链接。提交前请确认：

- [ ] 该仓库目前仍可公开访问（未被 DMCA 下架）
- [ ] 该仓库为学习、研究、重写或工具性质，而非单纯转储
- [ ] 你已阅读并理解本列表顶部的免责声明

---

*最后更新：2026-04-04 | 仓库状态随时可能因 DMCA 请求而变化，请以实际访问结果为准。*
