# 技能熔炉 v5.1

> 锻造 → 评估，两入口全流程交付可自动触发、稳定输出的 Skill。v5.0 新增前置闸门 + 五类入口路由 + 一次一问 + 分层验证 + 最后一公里。

[![版本](https://img.shields.io/badge/version-5.1.0-blue)](https://github.com/EdwardWason/skill-forge)
[![许可证](https://img.shields.io/badge/license-MIT--0-green)](LICENSE)
[![ClawHub](https://img.shields.io/badge/ClawHub-skill--forge--ai-orange)](https://clawhub.ai/skills/skill-forge-ai)

## 两入口触发

| 触发词 | 场景 | 执行流程 |
|--------|------|---------|
| **技能熔炉** | 从零创建到评估全流程 | Phase 0→1→2 |
| **技能评估** / skill评估 / 评估技能 | 已有Skill，评估质量 | Phase 2（质量自评+差异化验证） |

> **发布不在本技能范围内**：当用户说"技能发布/发布技能/更新技能/迭代技能"时，应触发独立的 **skill-publisher** 技能。

## 功能

- **同类预检（Step 0.4）**: 确认门通过后立即搜索SkillHub，避免重复造轮子。四种分支：有更好的→建议安装；有但不够好→提取差异点；无同类→直接创建；可组合→元技能组合+管线编排建议
- **前置闸门**：动手前判断"值不值得做/有没有现成的/该不该拆"，该劝退就劝退
- **五类入口路由**：从零想法/从对话提取/从现成材料/从草稿完善/改进已有skill
- **水平自适应**：从用户措辞自动判断水平，调整用词深度，全程不问"你几级"
- **一次一问**：每轮只问1个问题+2-3个选项，不再一次甩3个问题
- **确认门**：四要素问齐后回述一页小结，"这样对吗？"获批才动手写
- **自适应访谈**：2-5轮渐进式访谈，行为追问+偏误检测+选项法，精准锁定需求
- **三条铁律**：Description先行 / 一Skill一职 / 渐进式披露（≤200行+三级拆分），确保自动触发可靠
- **4+1模块结构**：任务/输出格式/规则/示例/故障排除(可选)，符合腾讯 Skills 手册规范
- **分层验证**：默认轻量"跑给你看"，老手/严谨场景才上重型量化评测
- **SkillHub同类比对**：搜索Top3同类Skill，腾讯9维度合规比对，差异化分析
- **渐进式披露**：SKILL.md≤200行只放导航，详细内容拆入 references/ + scripts/ + assets/
- **Frontmatter扩展**：allowed-tools / model / effort / metadata，精准控制工具权限和思考深度
- **触发描述优化迭代**：初版description写完后用5条真实用户说法测试，不准就迭代
- **最后一公里**：告诉用户skill放哪、怎么确认装上、怎么自测触发、怎么打包分发

## 快速开始

```bash
# ClawHub 安装
clawhub install skill-forge-ai

# 或手动安装
git clone https://github.com/EdwardWason/skill-forge.git
cp -r skill-forge ~/.trae/skills/
```

## 使用方法

在 TRAE SOLO 中，当你说以下内容时，Skill Forge 会自动触发：

- "技能熔炉" — 全流程（前置闸门→入口路由→访谈→确认门→同类预检→创建→验证→评估→发布+最后一公里）
- "技能评估" — 只做9维度质量自评+差异化验证+盲区修复
- "技能发布" — 只做GitHub+ClawHub推送+安装自测

### 两种模式

1. **上下文充足模式**：如果你已经和AI有多轮对话且创建要素齐全，直接进入创建流程
2. **访谈模式**：新任务启动时，通过2-5轮自适应访谈逐步锁定需求

### 创建流程

```
Phase -1: 前置闸门 → 值不值得做 → 有没有现成 → 该不该拆
Phase 0: 入口路由 → 五类入口 → 自适应访谈(一次一问) → 确认门 → 同类预检(Step 0.4)
Phase 1: 创建 → Description先行+触发优化 → 4+1模块 → 分层验证
Phase 2: 质量自评 → 腾讯9维度自评 → 差异化验证 → 盲区修复
Phase 3: 发布+最后一公里 → 安全审查 → 推送 → 安装自测
```

评估完成后，本技能会主动提示用户调用 skill-publisher 进行发布。

## 文件结构

```
skill-forge/
├── SKILL.md                              # 主入口（≤200行，只放导航信息）
├── references/
│   ├── interview-flow.md                 # 访谈流程详细参考
│   ├── interview-methods.md              # 访谈方法论深度参考
│   ├── benchmarking-guide.md             # SkillHub比对指南
│   ├── pre-gate-and-routing.md           # 前置闸门+五类入口路由+改进诊断
│   ├── composition-and-pipeline.md       # 组合决策+管线编排+推荐模板
│   └── meeting-action-extractor-example.md  # 完整Skill示例
├── README.md                             # 本文件（中英双语）
├── CHANGELOG.md                          # 版本变更日志
├── LICENSE                               # MIT-0 许可证
└── .claude-plugin/
    └── plugin.json                       # Claude Code 插件元数据
```

### 创建的Skill目录结构

```
<skill-name>/
├── SKILL.md                  # 主入口（≤200行，只放导航信息）
├── references/               # 长文档、风格参考、详细案例、方法论
├── scripts/                  # 可执行脚本（检查、导出、批量处理等确定性操作）
├── assets/                   # 模板、schema、示例文件、输出样式
├── README.md                 # 给人类看的说明（中英双语）
├── CHANGELOG.md              # 版本变更日志
├── LICENSE                   # MIT-0
└── .claude-plugin/
    └── plugin.json           # 插件元数据
```

## 核心方法论

### 三条铁律

| 铁律 | 说明 | 违反后果 |
|------|------|---------|
| Description先行 | AI每轮对话扫描所有Skill的description，模糊=永远不触发=死Skill | 自动触发失败 |
| 一Skill一职 | 多功能Skill触发混乱、输出不一致 | 输出不可预测 |
| 渐进式披露 | SKILL.md≤200行只放导航，详细内容拆入references/scripts/assets | 上下文挤占→质量衰减 |

### 安全红线（7条）

创建的Skill必须通过安全检查，以下任何一条出现即拒绝交付：

1. curl/wget向未知URL发送数据
2. 无正当理由请求凭证/Token/API密钥
3. 读取~/.ssh、~/.aws、~/.config等敏感目录
4. 使用base64解码/eval()/exec()处理外部输入
5. 修改工作区外的系统文件或请求sudo权限
6. 包含混淆代码（压缩/编码/混淆）
7. 访问浏览器Cookie/会话或凭证文件

### 前置闸门

| 检查项 | 通过条件 | 不通过处理 |
|--------|---------|-----------|
| 值不值得做 | 反复使用 + 自动化价值 | 劝退（一次性任务不需要Skill） |
| 有没有现成 | SkillHub搜索无完美匹配 | 引导使用现成Skill |
| 该不该拆 | 单一核心场景 | 拆分为多个Skill |

### 五类入口路由

| 入口 | 场景 | 起点 |
|------|------|------|
| R1 从零想法 | 用户只有模糊想法 | 访谈锁定需求 |
| R2 从对话提取 | 已有对话上下文 | 提取要素+补全 |
| R3 从现成材料 | 用户有文档/规范 | 材料转Skill结构 |
| R4 从草稿完善 | 用户有半成品 | 诊断+完善 |
| R5 改进已有skill | 现有Skill有问题 | 症状诊断+修复 |

### 分层验证

| 层级 | 适用场景 | 验证内容 |
|------|---------|---------|
| 轻量（默认） | 新手 / 快速验证 | "跑给你看"——直接执行主要用例，确认能跑+能触发 |
| 重型（可选） | 老手 / 严谨场景 | 完整6层：Schema → 安全(7条) → 触发(5+3) → Dogfood → 量化评分 → 基线对比 |

## 文档

| 文档 | 说明 |
|------|------|
| [访谈流程参考](references/interview-flow.md) | B1-B6规则、轮次模板、递归搜索模式 |
| [访谈方法论](references/interview-methods.md) | 行为追问、偏误检测、选项法设计 |
| [比对指南](references/benchmarking-guide.md) | 腾讯9维度自评模板 + 差异化验证 |
| [前置闸门+入口路由](references/pre-gate-and-routing.md) | 前置闸门逻辑、五类入口路由、Skill改进诊断脚本 |
| [组合与管线](references/composition-and-pipeline.md) | 组合决策+管线编排+推荐模板 |
| [完整示例](references/meeting-action-extractor-example.md) | 会议行动项提取器Skill示例 |

## 与 skill-publisher 的关系

本技能专注于 Skill 的**锻造与评估**（Phase 0-2）。当评估完成、用户准备发布时，本技能会主动提示调用 **skill-publisher** 技能完成发布流程：

- 仓库结构生成
- 安全审查（含扩展凭证模式）
- 版本号查重
- GitHub 推送
- ClawHub 发布
- 发布后验证

说"技能发布"或"发布技能"即可触发 skill-publisher。

## License

MIT-0 © 2026 AI花生

---

# Skill Forge (技能熔炉) v5.1

> Forge → Evaluate, two-entry pipeline delivering Skills that auto-trigger reliably and produce stable, structured output. v5.0 adds Pre-Gate + Five Entry Routes + One-Question-at-a-Time + Layered Validation + Last Mile.

[![Version](https://img.shields.io/badge/version-5.1.0-blue)](https://github.com/EdwardWason/skill-forge)
[![License](https://img.shields.io/badge/license-MIT--0-green)](LICENSE)
[![ClawHub](https://img.shields.io/badge/ClawHub-skill--forge--ai-orange)](https://clawhub.ai/skills/skill-forge-ai)

## Two-Entry Triggers

| Trigger Words | Scenario | Pipeline |
|---------------|----------|----------|
| **技能熔炉** | Create from scratch to evaluation | Phase 0→1→2 |
| **技能评估** / skill评估 / 评估技能 | Evaluate existing Skill | Phase 2 (Quality self-assessment + Differentiation validation) |

> **Publishing is out of scope**: When users say "技能发布/发布技能/更新技能/迭代技能", the standalone **skill-publisher** skill should be triggered instead.

## Features

- **Peer Pre-check (Step 0.4)**: Search SkillHub immediately after confirmation gate to avoid reinventing the wheel. Four branches: better exists → recommend install; exists but insufficient → extract differentiators; no peers → create directly; composable → meta-skill composition + pipeline orchestration
- **Pre-Gate**: Judges "worth doing? / already exists? / too big?" before investing time. Dissuades one-time tasks.
- **Five Entry Routes**: R1 from scratch / R2 from conversation / R3 from existing material / R4 from draft / R5 improve existing skill
- **Level Adaptation**: Auto-detects user level from language. Never asks "你几级". Adapts terminology depth in real-time.
- **One Question at a Time**: Each interview round asks only 1 question + 2-3 options. No more 3-questions-at-once.
- **Confirmation Gate**: After 4 elements gathered, present one-page summary. "理解没对齐，绝不动手写。"
- **Adaptive Interview**: 2-5 round progressive interview with behavioral probing, bias detection, and option-first design
- **Three Iron Rules**: Description-first / One-Skill-One-Job / Progressive Disclosure (≤200 lines + 3-tier split), ensuring reliable auto-triggering
- **4+1 Module Structure**: Task / Output Format / Rules / Example / Troubleshooting (optional), compliant with Tencent Skills Manual
- **Layered Validation**: Default lightweight "run it for you" for beginners; optional heavy 6-layer validation for power users
- **SkillHub Peer Benchmarking**: Search Top 3 peers, 9-dimension Tencent Manual compliance comparison, differentiation analysis
- **Progressive Disclosure**: SKILL.md ≤200 lines (navigation only), details split into references/ + scripts/ + assets/
- **Extended Frontmatter**: allowed-tools / model / effort / metadata for precise tool permission and thinking depth control
- **Description Optimization Iteration**: After writing initial description, test with 5 real user phrases. Auto-iterate if trigger accuracy is low
- **Last Mile**: Tells user where the skill is installed, how to verify, how to self-test trigger, how to package and distribute

## Quick Start

```bash
# Install via ClawHub
clawhub install skill-forge-ai

# Or manual install
git clone https://github.com/EdwardWason/skill-forge.git
cp -r skill-forge ~/.trae/skills/
```

## Usage

Skill Forge auto-triggers when you say:
- "技能熔炉" — Full pipeline (pre-gate → entry routing → interview → confirmation gate → peer pre-check → creation → validation → evaluation → publish + last mile)
- "技能评估" — Evaluation only (9-dimension quality self-assessment + differentiation validation + blind spot fix)
- "技能发布" — Publish only (GitHub + ClawHub push + install self-test)

### Two Modes

1. **Context-rich mode**: Skip interview if 4+ essential elements are already present in conversation
2. **Interview mode**: 2-5 round adaptive interview to progressively lock down requirements

### Pipeline

```
Phase -1: Pre-Gate → Worth doing? → Already exists? → Should split?
Phase 0: Entry routing → Five entry routes → Adaptive interview (one question at a time) → Confirmation gate → Peer pre-check (Step 0.4)
Phase 1: Creation → Description-first + trigger optimization → 4+1 modules → Layered validation
Phase 2: Quality self-assessment → Tencent 9-dimension self-assessment → Differentiation validation → Blind spot fix
Phase 3: Publish + last mile → Security audit → Push → Install self-test
```

After evaluation completes, this skill prompts the user to invoke skill-publisher for publishing.

## File Structure

```
skill-forge/
├── SKILL.md                              # Main entry (≤200 lines, navigation only)
├── references/
│   ├── interview-flow.md                 # Interview flow reference
│   ├── interview-methods.md              # Interview methodology
│   ├── benchmarking-guide.md             # SkillHub benchmarking guide
│   ├── pre-gate-and-routing.md           # Pre-gate + five entry routes + improvement diagnosis
│   ├── composition-and-pipeline.md       # Composition decisions + pipeline orchestration + templates
│   └── meeting-action-extractor-example.md  # Full Skill example
├── README.md                             # This file (bilingual)
├── CHANGELOG.md                          # Version changelog
├── LICENSE                               # MIT-0
└── .claude-plugin/
    └── plugin.json                       # Plugin metadata
```

### Created Skill Directory Structure

```
<skill-name>/
├── SKILL.md                  # Main entry (≤200 lines, navigation only)
├── references/               # Long docs, style guides, detailed cases, methodology
├── scripts/                  # Executable scripts (checks, exports, batch processing)
├── assets/                   # Templates, schemas, example files, output styles
├── README.md                 # Human-readable docs (bilingual)
├── CHANGELOG.md              # Version changelog
├── LICENSE                   # MIT-0
└── .claude-plugin/
    └── plugin.json           # Plugin metadata
```

## Core Methodology

### Three Iron Rules

| Rule | Description | Consequence of Violation |
|------|-------------|--------------------------|
| Description-first | AI scans all Skill descriptions every conversation; vague = never triggers = dead Skill | Auto-trigger failure |
| One-Skill-One-Job | Multi-purpose Skills trigger chaotically and output inconsistently | Unpredictable output |
| Progressive Disclosure | SKILL.md ≤200 lines (navigation only); details split into references/scripts/assets | Context bloat → quality decay |

### Security Red Lines (7 items)

Any created Skill must pass security check. Any red flag below = reject:

1. curl/wget to unknown URLs or sending data to external servers
2. Requesting credentials/tokens/API keys without clear reason
3. Reading ~/.ssh, ~/.aws, ~/.config, MEMORY.md, USER.md, IDENTITY.md
4. Using base64 decode / eval() / exec() with external input
5. Modifying system files outside workspace or requesting sudo
6. Obfuscated code (compressed, encoded, minified)
7. Accessing browser cookies/sessions or credential files

### Pre-Gate

| Check | Pass condition | Fail action |
|-------|---------------|-------------|
| Worth doing? | Repeated use + automation value | Dissuade (one-time tasks don't need Skills) |
| Already exists? | No perfect match on SkillHub | Guide to existing Skill |
| Should split? | Single core scenario | Split into multiple Skills |

### Five Entry Routes

| Route | Scenario | Starting point |
|-------|----------|----------------|
| R1 From scratch | User has only a vague idea | Interview to lock down requirements |
| R2 From conversation | Existing conversation context | Extract elements + fill gaps |
| R3 From existing material | User has docs/specs | Convert material to Skill structure |
| R4 From draft | User has a half-finished draft | Diagnose + improve |
| R5 Improve existing skill | Existing Skill has issues | Symptom diagnosis + fix |

### Layered Validation

| Level | When to use | What to validate |
|-------|-------------|-----------------|
| Lightweight (default) | Beginners / quick check | "Run it for you" — execute main use cases, confirm it runs + triggers |
| Heavy (optional) | Power users / rigorous scenarios | Full 6-layer: Schema → Security (7) → Trigger (5+3) → Dogfood → Scoring → Baseline |

## Documentation

| Document | Description |
|----------|-------------|
| [Interview Flow](references/interview-flow.md) | B1-B6 rules, round templates, recursive search pattern |
| [Interview Methods](references/interview-methods.md) | Behavioral probing, bias detection, option design |
| [Benchmarking Guide](references/benchmarking-guide.md) | Tencent 9-dimension self-assessment template + differentiation validation |
| [Pre-Gate & Routing](references/pre-gate-and-routing.md) | Pre-gate logic, five entry routes, skill improvement diagnosis |
| [Composition & Pipeline](references/composition-and-pipeline.md) | Composition decisions + pipeline orchestration + templates |
| [Full Example](references/meeting-action-extractor-example.md) | Meeting action extractor Skill example |

## Relationship with skill-publisher

This skill focuses on **forging and evaluating** Skills (Phase 0-2). When evaluation is complete and the user is ready to publish, this skill prompts the user to invoke **skill-publisher** to handle the publishing pipeline:

- Repository structure generation
- Security audit (with extended credential patterns)
- Version deduplication check
- GitHub push
- ClawHub publishing
- Post-publish verification

Say "技能发布" or "发布技能" to trigger skill-publisher.

## License

MIT-0 © 2026 AI花生
