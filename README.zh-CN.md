# AI Agent Book Skill

[English](./README.md) · [简体中文](./README.zh-CN.md) 

> 将李博杰（Bojie Li）所著《深入理解 AI Agent》一书蒸馏为一组可直接调用的 AI Agent 技能（Skills）。
>
> Distills the book *"Deep Understanding of AI Agents"* by Li Bojie into a collection of ready-to-use AI Agent skills.

本书英文版在线阅读：[https://bojieli.com/ai-agent-book/](https://github.com/bojieli/ai-agent-book)

---

## 这是什么

本仓库使用两种开源 Skill 提取工具，对《深入理解 AI Agent》的**英文版**（`book-en/`，共 10 章 + 引言）进行了方法论蒸馏，产出了 **1 个综合型 Skill + 25 个原子化方法论 Skill**。

每个原子 Skill 遵循 **RIA++ 结构**（Reading 原文引用 → Interpretation 方法论解读 → A1 书中实例 → A2 触发场景 → Execution 执行步骤 → Boundary 边界条件），配有一套 `test-prompts.json` 测试用例（含诱饵测试和跨 Skill 混淆测试），可直接被 Hermes、Claude Code、GitHub Copilot CLI 等 AI Agent 工具加载使用。

### 使用的提取工具

| 工具 | 仓库 | 产出 |
|------|------|------|
| **[cangjie-skill](https://github.com/kangarooking/cangjie-skill)** | kangarooking/cangjie-skill | 25 个原子化方法论 Skill（RIA-TV++ 流水线：Adler 整书理解 → 5 路并行提取 → 三重验证 → RIA++ 构造 → Zettelkasten 链接 → 压力测试） |
| **[book-to-skill](https://github.com/virgiliojr94/book-to-skill)** | virgiliojr94/book-to-skill | 1 个综合型 Skill（含 10 个章节文件 + 术语表 + 设计模式表 + 决策速查表） |

---

## 覆盖度

对原书 10 章共 **100 个章节段落** 逐项核查：

| 状态 | 占比 | 说明 |
|------|------|------|
| ✅ 完全覆盖 | **89%** | 89/100 段——方法论、框架、原则已被提取为 Skill |
| ⚠️ 部分覆盖 | 11% | 11/100 段——API 机制层细节（如 Chat Template 的 tokenization 流程）、前沿实验参数等非方法论内容 |

三大核心论点已与原书英文版**逐字比对确认**：

- ✅ **Harness = Constrain + Verify + Correct**（第 1 章）
- ✅ **SFT memorizes, RL generalizes**（第 7 章）
- ✅ **当差异 < 噪声带宽时不要切换模型**（第 6 章）

---

## 目录结构

```
ai-agent-book-skill/
│
├── ai-agent-book/                    ← 综合型 Skill（book-to-skill 产出）
│   ├── SKILL.md                      主文件：核心框架速览 + 章节索引 + 主题索引
│   ├── chapters/
│   │   ├── ch01-getting-started.md   第 1 章：AI Agent 入门
│   │   ├── ch02-context-engineering.md 第 2 章：上下文工程
│   │   ├── ch03-memory-knowledge.md  第 3 章：用户记忆与知识库
│   │   ├── ch04-tools.md             第 4 章：工具
│   │   ├── ch05-coding-agent.md      第 5 章：编程 Agent 与代码生成
│   │   ├── ch06-evaluation.md        第 6 章：评测
│   │   ├── ch07-post-training.md     第 7 章：模型后训练
│   │   ├── ch08-self-evolution.md    第 8 章：Agent 自进化
│   │   ├── ch09-multimodal.md        第 9 章：多模态与实时交互
│   │   └── ch10-multi-agent.md       第 10 章：多 Agent 协作
│   ├── glossary.md                   术语表（70+ 术语）
│   ├── patterns.md                   设计模式表（19 个模式）
│   └── cheatsheet.md                 决策速查表（权衡矩阵 + 阈值 + if/then 规则）
│
├── cangjie-distilled/                ← 第 1-5 章原子 Skill（cangjie-skill 产出）
│   ├── BOOK_OVERVIEW.md              Adler 整书分析
│   ├── INDEX.md                      Skill 总览 + 引用关系图
│   ├── GLOSSARY.md                   共享术语词典
│   ├── DIGEST.md                     精华长文（~5400 词）
│   ├── harness-engineering/          每个 Skill 一个目录
│   │   ├── SKILL.md                  RIA++ 六段结构
│   │   └── test-prompts.json         测试用例（3 正例 + 2 诱饵 + 1 边界）
│   ├── kv-cache-friendly-context/
│   ├── ... (共 13 个 Skill)
│
├── cangjie-ch6-10/                   ← 第 6-10 章原子 Skill（cangjie-skill 产出）
│   ├── BOOK_OVERVIEW_CH6_10.md
│   ├── INDEX_CH6_10.md
│   ├── GLOSSARY_CH6_10.md
│   ├── DIGEST_CH6_10.md
│   ├── sft-memorizes-rl-generalizes/
│   │   ├── SKILL.md
│   │   └── test-prompts.json
│   └── ... (共 12 个 Skill)
│
├── .gitignore
├── LICENSE                           MIT
├── README.md                         本文件（中文）
└── README.en.md                      English README
```

---

## 提取的 Skill 列表

### 综合型 Skill（book-to-skill 产出）

| Skill | 内容 |
|-------|------|
| **ai-agent-book** | 全书 10 章结构化知识：核心框架速览 + 按需加载的章节文件 + 术语表 + 设计模式 + 决策速查 |

### 原子化方法论 Skill（cangjie-skill 产出，共 25 个）

#### 第 1-5 章（架构与基础设施）

| Skill | 标题 | 章节 |
|-------|------|------|
| `harness-engineering` | Harness 工程：约束、验证、纠正 | Ch1 |
| `engineering-paradigm-evolution` | 五层工程范式演进 | Ch1 |
| `kv-cache-friendly-context` | KV Cache 友好的上下文设计 | Ch2 |
| `progressive-disclosure-skills` | 通过 Agent Skill 实现渐进式披露 | Ch2 |
| `context-compression-strategy` | 上下文压缩策略选择 | Ch2 |
| `agent-status-bar` | Agent 状态栏：用元信息管理轨迹 | Ch2 |
| `three-level-memory-evaluation` | 三级记忆评估框架 | Ch3 |
| `agentic-rag` | Agentic RAG：从被动管线到主动探索 | Ch3 |
| `two-tier-memory-architecture` | 双层记忆架构 | Ch3 |
| `tool-design-principles` | 工具设计原则（Agent-Computer Interface） | Ch4 |
| `proactive-tool-discovery` | 主动工具发现 | Ch4 |
| `coding-agent-meta-capability` | 编程 Agent 作为元能力 | Ch5 |
| `proposer-reviewer-pattern` | 提议者-审查者模式 | Ch1/4/5 |

#### 第 6-10 章（评测、训练、进化、多模态、多 Agent）

| Skill | 标题 | 章节 |
|-------|------|------|
| `three-level-evaluation-system` | 三级评测体系 | Ch6 |
| `model-swap-vs-ablation` | 模型交换 vs 消融实验：诊断瓶颈 | Ch6 |
| `pass-at-k-vs-pass-k-k` | Pass@k vs Pass^k：能力天花板 vs 可靠性 | Ch6 |
| `statistical-significance-eval` | 统计显著性：不要在噪声上做决策 | Ch6 |
| `sft-memorizes-rl-generalizes` | SFT 记忆，RL 泛化 | Ch7 |
| `data-environment-over-algorithms` | 数据和环境比算法更重要 | Ch7 |
| `process-vs-outcome-reward` | 过程奖励 vs 结果奖励设计 | Ch7 |
| `verification-generation-asymmetry` | 验证-生成不对称性 | Ch7 |
| `externalized-learning-three-products` | 外化学习：三种产物 | Ch8 |
| `sleep-learning-memory-consolidation` | 睡眠学习：用户记忆自主进化 | Ch8 |
| `fast-slow-thinking-separation` | 快慢思维分离 | Ch9 |
| `multi-agent-new-information-criterion` | 多 Agent 新信息判据 | Ch10 |

---

## 如何使用这些 Skill

### 方式一：让 AI 工具自动安装（推荐）

将仓库地址发给你的 AI Agent（Hermes / Claude Code / GitHub Copilot CLI 等），让它自动拉取并安装：

> **仓库地址：https://github.com/Samge0/ai-agent-book-skill**

示例对话：
```
请从 https://github.com/Samge0/ai-agent-book-skill 克隆这个技能仓库，
将 ai-agent-book/ 安装为综合 Skill，将 cangjie-distilled/ 和 cangjie-ch6-10/ 
下的各个子目录安装为独立 Skill。
```

### 方式二：Hermes 手动安装

```bash
# 克隆仓库
git clone https://github.com/Samge0/ai-agent-book-skill.git

# 将 Skill 目录复制到 Hermes skills 目录
# 综合型 Skill
cp -r ai-agent-book-skill/ai-agent-book ~/.hermes/skills/

# 原子化 Skill（每个子目录都是一个独立 Skill）
cp -r ai-agent-book-skill/cangjie-distilled/harness-engineering ~/.hermes/skills/
cp -r ai-agent-book-skill/cangjie-distilled/kv-cache-friendly-context ~/.hermes/skills/
# ... 按需复制其他 Skill
```

Hermes 的 skills 目录通常在：
- **Linux / macOS / WSL**：`~/.hermes/skills/` 或 `~/AppData/Local/hermes/skills/`
- **Windows**：`C:\Users\<用户名>\AppData\Local\hermes\skills\`

### 方式三：Claude Code 手动安装

```bash
git clone https://github.com/Samge0/ai-agent-book-skill.git

# 将 Skill 目录复制/链接到 Claude Code skills 目录
cp -r ai-agent-book-skill/ai-agent-book ~/.claude/skills/
cp -r ai-agent-book-skill/cangjie-distilled/harness-engineering ~/.claude/skills/
# ... 按需复制
```

### 方式四：GitHub Copilot CLI 手动安装

```bash
git clone https://github.com/Samge0/ai-agent-book-skill.git

# Copilot CLI skills 目录
cp -r ai-agent-book-skill/ai-agent-book ~/.copilot/skills/
```

### 使用示例

安装后，AI Agent 会根据你的问题自动加载对应的 Skill。例如：

- 你问 *"我的 Agent 总是死循环，怎么让它可靠一点？"* → 自动加载 `harness-engineering`
- 你问 *"SFT 和 RL 我该选哪个？"* → 自动加载 `sft-memorizes-rl-generalizes`
- 你问 *"新模型比旧模型高了 3%，要切换吗？"* → 自动加载 `statistical-significance-eval`
- 你问 *"如何让我的 Agent 跨会话记住用户偏好？"* → 自动加载 `sleep-learning-memory-consolidation`

---

## 每个 Skill 的内部结构（RIA++）

每个原子化 Skill 的 `SKILL.md` 遵循以下六段结构：

| 段落 | 含义 |
|------|------|
| **R — Reading** | 原书原文引用（≤100 词），附章节出处 |
| **I — Interpretation** | 方法论骨架解读（用自己的话，不照抄原文） |
| **A1 — Past Application** | 书中实例：作者（或业界）如何使用这个框架 |
| **A2 — Future Trigger** | 触发场景：用户什么情况下需要这个 Skill + 语言信号 |
| **E — Execution Steps** | 执行步骤：每步有明确的完成标准 |
| **B — Boundary** | 边界条件：什么时候**不该**用这个 Skill |

每个 Skill 还配有 `test-prompts.json`（darwin-skill 兼容格式），包含：
- 3 条 `should_trigger` 正例
- 2 条 `should_not_trigger` 诱饵（其中至少 1 条是跨 Skill 混淆测试）
- 1 条 `edge_case` 边界用例

---

## 致谢

- **原书作者**：[李博杰（Bojie Li）](https://github.com/bojieli)，Pine AI 首席科学家
- **原书仓库**：[bojieli/ai-agent-book](https://github.com/bojieli/ai-agent-book)（多种语言版本）
- **提取工具 1**：[cangjie-skill](https://github.com/kangarooking/cangjie-skill) by kangarooking — RIA-TV++ 蒸馏流水线
- **提取工具 2**：[book-to-skill](https://github.com/virgiliojr94/book-to-skill) by virgiliojr94 — 文档结构化转换工具

---

## License

[MIT](LICENSE)
