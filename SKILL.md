---
name: "skill-forge"
slug: "skill-forge-ai"
displayName: "Skill Forge"
description: "技能熔炉 — 锻造/评估 Skill。说 技能熔炉 走全流程；说 技能评估/skill评估/评估技能 只做同类比对+腾讯9维度。发布环节请用 skill-publisher。Do NOT use for editing existing skills, skill security vetting, general coding tasks, or skill publishing (use skill-publisher)."
version: "4.3.0"
license: "MIT"
allowed-tools: "Bash(mkdir:*), Bash(curl:*), Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion"
---

# 技能熔炉 v4.3

锻造 → 评估，两入口全流程交付可自动触发、稳定输出的 Skill。发布环节由独立的 skill-publisher 技能承接。

## 入口检测

**根据用户触发词，决定从哪个阶段开始：**

| 触发词 | 入口 | 执行流程 |
|--------|------|---------|
| 技能熔炉 | Phase 0 | Phase 0→1→2 全流程 |
| 技能评估 / skill评估 / 评估技能 | Phase 2 | 只做 SkillHub 同类比对 + 腾讯9维度 |

**检测到触发词后，立即跳转到对应 Phase，不执行前面的阶段。**

**发布不在本技能范围内**：当用户说"技能发布/发布技能/更新技能/迭代技能"时，应触发 skill-publisher，不是本技能。

## 三条铁律

违反任何一条 = 废技能。

**铁律1：Description先行** — AI每轮对话扫描所有Skill的description，模糊=永远不触发=死Skill。

**铁律2：一Skill一职** — 不要把多个场景塞进一个Skill，多功能Skill触发混乱、输出不一致。

**铁律3：渐进式披露** — SKILL.md ≤200行，只放导航信息（触发/原则/步骤/验证）。详细内容按三级拆分：
- `references/` — 长文档、风格参考、详细案例
- `scripts/` — 可执行脚本（确定性操作用脚本比让模型现场生成更稳定）
- `assets/` — 模板、schema、示例文件、输出样式

## SKILL.md 格式

```markdown
---
name: "<skill-name>"
description: "<做什么 + 何时触发。核心关键词放前200字符>"
allowed-tools: "<工具白名单，如：Bash(python:*) WebFetch>"
model: "<推荐模型，如：claude-opus-4-5>"
effort: "<思考深度：low/medium/high>"
metadata:
  author: "<作者>"
  version: "<版本>"
  category: "<分类>"
---

# <技能标题>

## 任务
<一句话：只做X，不做Y和Z>

## 输出格式
<固定输出结构。每个字段格式必须具体，绝不写"整理清晰">

## 规则
<3-5条硬规则。每条必须通过实习生测试——实习生能直接执行>

## 示例
<一组完整的输入输出，覆盖边界情况>

## 故障排除（可选）
<常见错误 + 原因 + 解决方案>
```

## 必填字段

| 字段 | 位置 | 必填 | 说明 |
|------|------|------|------|
| `name` | frontmatter | **是** | kebab-case，唯一标识 |
| `description` | frontmatter | **是** | (1)做什么 + (2)何时触发 + (3)Do NOT范围。200字符以内。关键词前置。 |
| `allowed-tools` | frontmatter | 推荐 | 工具白名单。足够但不过度。 |
| `model` | frontmatter | 可选 | 推荐模型。简单任务用Haiku省钱，复杂决策用Opus换准确率。 |
| `effort` | frontmatter | 可选 | 思考深度控制。low省钱省时，high换准确率。 |
| `metadata` | frontmatter | 推荐 | author / version / category 等。 |

## 目录结构

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

---

## Phase 0: 意图识别与自适应访谈

**【入口：技能熔炉】** — 读取 [`references/interview-flow.md`](references/interview-flow.md) 获取完整访谈方法论。

### Step 0.1: 要素检查

扫描上下文中的5个关键要素：**单一场景 / 触发条件 / 输出格式 / 范围边界 / 硬约束**。

- **≥4个齐备** → 与用户确认，跳到 Phase 1
- **<4个齐备** → 进入自适应访谈（Step 0.2）

### Step 0.2: 自适应访谈（2-5轮）

每轮用选项优先问题（AskUserQuestion，3个强选项+Other）+行为追问。每轮后更新要素清单，**≥4个明确 → 进入 Phase 1**。最多5轮。

---

## Phase 1: 创建

**【入口：技能熔炉】**

### Step 1: Description先行

**格式**: `"<做什么>. 当用户说<具体触发词>时触发. Do NOT use for <排除范围>."`

**截断机制**: 核心触发关键词必须在前200字符内。尾部在~250字符处截断。

### Step 2: 撰写4+1模块内容

**任务**: 锁定边界。声明"做X"和"不做Y"。
**输出格式**: 固定输出结构。每个字段必须有具体格式，绝不写模糊指令。
**规则**: 仅3-5条。必须通过**实习生测试**。删除废话规则。
**示例**: 一组完整的输入输出。一个好示例 > 10条抽象规则。
**故障排除**（可选）: 常见错误 + 原因 + 解决方案。让Agent遇到问题时能自修复。

### Step 3: 创建目录和文件

按目录结构创建。判断是否需要 `scripts/` 和 `assets/`：
- 有确定性操作（检查、导出、批量处理）→ 创建 `scripts/`
- 有模板、样式、示例文件 → 创建 `assets/`

### Step 4: 自测验证流水线

**Step 4a: Schema检查** — name+description ✅ | <200字符 ✅ | 关键词前置 ✅ | Do NOT ✅ | 4模块 ✅ | 实习生测试 ✅ | ≤200行 ✅ | 示例含边界 ✅

**Step 4a+1: 信任检查（T维度）** — 三类扫描，任何 RED FLAG 立即拒绝：

**安全红线**（原7条，保留）：
1. curl/wget 向未知URL发送数据
2. 无正当理由请求凭证/Token/API密钥
3. 读取 ~/.ssh、~/.aws、~/.config、MEMORY.md、USER.md、IDENTITY.md
4. 使用 base64解码/eval()/exec() 处理外部输入
5. 修改工作区外的系统文件或请求sudo权限
6. 包含混淆代码
7. 访问浏览器Cookie/会话或凭证文件

**最小权限校验**（TRACE T维度新增）：
- 检查 frontmatter `allowed-tools`，与任务无关的工具权限必须删除
- 判断标准：如果删除该工具权限，Skill 仍能完成任务 → 该权限多余
- 例：周报生成Skill请求文件系统写入权限 → RED FLAG

**国内可用性校验**（TRACE T维度新增）：
- Skill 依赖的外部 API/服务必须在国内网络环境下可访问
- 完全依赖海外 API 且无国内替代方案 → RED FLAG
- 中文交互完整性：输入输出必须支持中文，不能仅英文

**Step 4b: 触发测试** — 准备5条真实用户说法（含口语化、改写、模糊表达），3条不应触发的反向测试。每条标记 should_trigger: true/false。

**Step 4c: Dogfood压力测试（R维度）** — 三类输入 × 三项检查：

**三类输入测试**：
- 常见输入：示例中的标准输入，验证基本流程
- 边界输入：空输入、超长输入、格式异常输入，验证健壮性
- 复杂输入：真实世界的脏数据/模糊表述，验证容错性

**三项检查**（每类输入都要过）：
- 格式匹配：输出是否遵循固定格式 ✅/❌
- 规则合规：输出是否违反规则 ✅/❌
- 异常处理反馈（TRACE R维度新增）：遇到无法完成的情况时，是否清楚说明原因并给出后续建议，而不是返回空结果或乱码 ✅/❌
  - ✅ 好：PDF解析遇扫描件 → "该文件为扫描图片，无法提取文本，建议先进行OCR处理"
  - ❌ 差：同样场景 → 无报错，直接返回空结果

**Step 4d: 量化评分** — 对Dogfood结果按0-10打分：
- 0-2: 完全没完成任务
- 3-4: 勉强相关，漏掉关键要求
- 5-6: 基本可用，有明显问题
- 7-8: 质量稳定，少量细节可改
- 9-10: 非常符合预期

**Step 4e: 有效性验证（E维度）** — 三层验证：

**基线对比**（保留）：同一任务，不用Skill跑一次 vs 用Skill跑一次。如果无Skill已7分，有Skill仍7分，说明Skill无增益。

**可直接可用性**（TRACE E维度新增）：输出是否需要用户大量手动修正才能用？
- ✅ 好：公众号润色Skill输出后，用户少量检查即可直接发布
- ❌ 差：输出只改了语气，用户仍需重新梳理结构、补案例、删空话
- 判断标准：修正量 < 20% → 通过；修正量 > 50% → 不通过

**增量价值**（TRACE E维度新增）：Skill 是否提供了用户自己做不到的能力？
- 信息整合：跨多个来源整合信息，用户手动难以完成
- 判断建议：基于数据分析给出专业判断，不是简单格式转换
- 质量提升：输出质量明显高于无AI辅助的人工产出
- 如果 Skill 只是简单格式转换或关键词替换 → 增量价值不足，建议重新设计

**最多3次迭代。3次后建议"先发布V1再迭代"。**

---

## Phase 2: SkillHub 同类比对 + 腾讯9维度评估

**【入口：技能评估 / skill评估 / 评估技能】** — 读取 [`references/benchmarking-guide.md`](references/benchmarking-guide.md) 获取完整比对方法论。

### Step 5a: 搜索与排名

调用 SkillHub API: `https://api.skillhub.cn/api/v1/search?q=<keywords>`。按 `downloads × 0.4 + installs × 0.3 + stars × 0.3` 排名。取 Top 3。

### Step 5b: 腾讯手册9维度合规比对

与 Top 3 同类按9维度比对：Description触发精准度 / 关键词前置 / Do NOT范围 / 单一职责 / 4模块结构 / 输出格式具体性 / 实习生测试规则 / 示例边界覆盖 / 体积控制。

### Step 5c: 差异化与盲区分析

- 重复 → 建议安装已有Skill
- 有差异 → 明确记录差异化
- 有盲区 → 列出并附腾讯手册依据

### Step 5d: 用户决策

展示结果。用户选择：采纳修复 / 保持原样 / 安装已有。**用户决策为最终决策。**

---

## 发布交接提醒

**触发条件**（满足以下任一）：
1. 【技能熔炉入口】Step 4 验证全通过（≥7分）+ Step 5d 用户选择"保持原样"或"采纳修复且修复完成"
2. 【技能评估入口】Step 5d 用户选择"保持原样"或"采纳修复且修复完成"

**不触发**：
- Step 5d 用户选择"直接安装已有"（用户不需要发布自己的 Skill）
- Step 4 验证未通过（评分 <7）
- Step 5c 发现有 Critical 盲区未修复

**触发时提示**：

> Skill 已通过锻造与评估。如需发布到 GitHub + ClawHub + SkillHub，请说"技能发布"或"发布技能"调用 **skill-publisher** 技能，它负责：前置条件校验 → 仓库结构生成 → 安全审查 → 版本号查重 → 三平台推送 → 发布后验证。

**不要在本技能内执行任何发布操作。** 发布是独立技能 skill-publisher 的职责，本技能仅负责锻造与评估。

---

## 示例

### 示例1：常见输入（信息充足，直接创建）

**用户输入**："帮我做一个技能，自动从会议纪要中提取行动项、决策和待办问题，输出固定格式的表格。当用户发来会议纪要或转写稿时触发。不做会议录音转写，不做任务跟踪。"

**要素检查**：5项齐备（单一场景✅ / 触发条件✅ / 输出格式✅ / 范围边界✅ / 硬约束✅）→ 跳过访谈，直接 Phase 1

**Phase 1 输出**：
```yaml
name: "meeting-action-extractor"
description: "Extracts action items, decisions, and pending questions from meeting transcripts. Invoke when user sends meeting notes, transcript, or asks to extract action items. Do NOT use for audio transcription, task tracking, or calendar scheduling."
```

任务/输出格式/规则/示例 4模块按规范生成，Step 4 验证全通过（评分8分）。

**Phase 2 评估**：SkillHub 搜索 Top 3 同类，9维度比对发现"示例未覆盖中文会议纪要"盲区 → 采纳修复 → 重新验证通过。

**发布交接提醒触发**：评估完成，用户选择"保持原样" → 提示调用 skill-publisher。

### 示例2：边界输入（信息不足，进入访谈）

**用户输入**："做个技能帮我整理笔记"

**要素检查**：2项齐备（单一场景✅ / 触发条件❌ / 输出格式❌ / 范围边界❌ / 硬约束❌）→ 进入自适应访谈

**Round 1**：
- Q1："你想让这个 Skill 帮你整理什么类型的笔记？" → 3选项：课堂笔记 / 读书笔记 / 会议笔记 + Other
- 用户选"会议笔记" → Why×1："为什么需要整理？上一次整理会议笔记时最头疼什么？"

**Round 2**：
- Q3："想想最近一次整理会议笔记的经过，一步步告诉我"（行为追问B1）
- 用户描述后 → 要素更新为4项齐备 → 进入 Phase 1

### 示例3：异常输入（需求矛盾）

**用户输入**："做个技能既能写公众号文章又能生成PPT还能剪视频"

**要素检查**：违反铁律2（一Skill一职）→ 标记矛盾

**处理**：引用用户原话，让用户选择优先级
> 你提到三个功能：写文章、生成PPT、剪视频。一个Skill只做一个场景（铁律2），请选择你最需要的一个：
> A. 公众号文章写作
> B. PPT生成
> C. 视频剪辑
> D. 其他

用户选A → 聚焦到公众号文章写作Skill，重新进入要素检查。

## References

- **[`references/interview-flow.md`](references/interview-flow.md)** — Phase 0 访谈方法论。B1-B6 规则、轮次模板、递归搜索、收敛检查。
- **[`references/interview-methods.md`](references/interview-methods.md)** — 访谈方法论深度参考。行为追问、偏误检测、选项法设计。
- **[`references/benchmarking-guide.md`](references/benchmarking-guide.md)** — Phase 2 比对方法论。SkillHub API、质量排序公式、腾讯9维度模板。
- **[`references/meeting-action-extractor-example.md`](references/meeting-action-extractor-example.md)** — 完整Skill示例。
