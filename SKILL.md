---
name: "skill-forge"
description: "技能熔炉 — 锻造高质量 Skill 的专业工具。当用户说 技能熔炉/熔炉创建/锻造技能/新建熔炉/熔炉技能 时立即触发。通过自适应访谈、递归搜索、自测验证、SkillHub同类比对锻造技能。Do NOT use for editing existing skills, skill security vetting, or general coding tasks."
---

# 技能熔炉 v3.4

锻造高质量 Skill 的专业熔炉——自适应访谈 → 创建 → 自测验证（含安全红线）→ SkillHub 同类比对，全流程交付可自动触发、稳定输出的 Skill。

## 何时触发

**关键：当用户说出以下词汇时，必须立即调用本技能：**
- "技能熔炉"
- "熔炉创建"
- "锻造技能"
- "新建熔炉"
- "熔炉技能"
- "用熔炉做一个技能"
- "帮我锻造一个skill"

**不要：**
- 在用户说"熔炉"相关词汇时，不调用本技能而只解释如何创建
- 将本技能与内置 skill-creator 混淆——本技能是升级版，含访谈、验证、比对

## 三条铁律

违反任何一条 = 废技能。

**铁律1：Description先行** — AI每轮对话扫描所有Skill的description，模糊=永远不触发=死Skill。

**铁律2：一Skill一职** — 不要把多个场景塞进一个Skill，多功能Skill触发混乱、输出不一致。

**铁律3：150行以内** — 超200行AI准确率下降，详细内容移入 `references/` 目录。

## SKILL 结构

1. **目录**: `.trae/skills/<skill-name>/`
2. **文件**: 目录内放 `SKILL.md`
3. **可选**: `references/` 放详细文档（保持SKILL.md精简）

## SKILL.md 格式

```markdown
---
name: "<skill-name>"
description: "<做什么 + 何时触发。核心关键词放前200字符>"
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
```

## 必填字段

| 字段 | 位置 | 说明 |
|------|------|------|
| `name` | frontmatter | 唯一标识 |
| `description` | frontmatter | **关键**：(1)做什么 + (2)何时触发 + (3)Do NOT范围。200字符以内。关键词前置。 |
| `detail` | body | frontmatter后的4模块内容 |

---

## Phase 0: 意图识别与自适应访谈

**进入 Step 0.2 时读取 [`references/interview-flow.md`](references/interview-flow.md) 获取完整访谈方法论。** 以下为摘要。

### Step 0.1: 要素检查

扫描上下文中的5个关键要素：**单一场景 / 触发条件 / 输出格式 / 范围边界 / 硬约束**。

- **≥4个齐备** → 与用户确认，跳到 Phase 1
- **<4个齐备** → 进入自适应访谈（Step 0.2）

### Step 0.2: 自适应访谈（2-5轮）

**立即读取 [`references/interview-flow.md`](references/interview-flow.md)**，包含：访谈规则(B1-B6)、轮次问题模板、递归搜索模式、收敛检查。

摘要：每轮用选项优先问题（AskUserQuestion，3个强选项+Other）+行为追问。每轮后更新要素清单，**≥4个明确 → 进入 Phase 1**。最多5轮。

---

## Phase 1: 创建

### Step 1: Description先行

**格式**: `"<做什么>. 当用户说<具体触发词>时触发. Do NOT use for <排除范围>."`

**截断机制**: 核心触发关键词必须在前200字符内。尾部在~250字符处截断。

### Step 2: 撰写4模块内容

**任务**: 锁定边界。声明"做X"和"不做Y"。

**输出格式**: 固定输出结构。每个字段必须有具体格式，绝不写模糊指令。

**规则**: 仅3-5条。必须通过**实习生测试**：实习生不能直接执行的规则，删掉。

删除废话规则："语言简洁" / "保持客观" / "排版整齐" / "代码清晰" / "遵循最佳实践" / "确保输出准确"

**示例**: 一组完整的输入输出。输入必须覆盖边界情况。一个好示例 > 10条抽象规则。

### Step 3: 创建目录和文件

```bash
mkdir -p .trae/skills/<skill-name>
```

### Step 4: 自测验证流水线

**Step 4a: Schema检查** — Frontmatter name+description ✅ | Description <200字符 ✅ | 触发关键词前置 ✅ | Do NOT范围 ✅ | 4模块 ✅ | 实习生测试 ✅ | <150行 ✅ | 示例含边界 ✅

**Step 4a+1: 安全红线检查** — 扫描创建的 SKILL.md，发现以下 RED FLAG 立即拒绝：
- curl/wget 向未知URL发送数据
- 无正当理由请求凭证/Token/API密钥
- 读取 ~/.ssh、~/.aws、~/.config、MEMORY.md、USER.md、IDENTITY.md
- 使用 base64解码/eval()/exec() 处理外部输入
- 修改工作区外的系统文件或请求sudo权限
- 包含混淆代码（压缩/编码/混淆）
- 访问浏览器Cookie/会话或凭证文件
通过 → 进入 Step 4b。有红线 → 移除，从 Step 4a 重新验证。

**Step 4b: 触发测试** — AI生成3个正向+3个反向假问题。正向不触发 → 加关键词。反向触发 → 加Do NOT。

**Step 4c: Dogfood模拟** — 用示例输入走一遍规则/格式。检查：格式匹配 ✅ | 规则合规 ✅ | 边界情况处理 ✅

**最多3次迭代。3次后建议"先发布V1再迭代"。**

---

## Phase 2: SkillHub 同类比对

**进入 Phase 2 时读取 [`references/benchmarking-guide.md`](references/benchmarking-guide.md) 获取完整比对方法论。** 以下为摘要。

### Step 5a: 搜索与排名

调用 SkillHub API: `https://api.skillhub.cn/api/v1/search?q=<keywords>`。按 `downloads × 0.4 + installs × 0.3 + stars × 0.3` 排名。取 Top 3。API不可用时用 CLI 降级。

### Step 5b: 腾讯手册合规比对

将我们的 Skill 与 Top 3 同类按腾讯9维度比对：Description触发精准度 / 关键词前置 / Do NOT范围 / 单一职责 / 4模块结构 / 输出格式具体性 / 实习生测试规则 / 示例边界覆盖 / 体积控制。

### Step 5c: 差异化与盲区分析

- 重复 → 建议安装已有Skill
- 有差异 → 明确记录差异化
- 有盲区 → 列出并附腾讯手册依据

### Step 5d: 用户决策

展示结果。用户选择：采纳修复 / 保持原样 / 安装已有。**用户决策为最终决策。**

---

## References

- **[`references/interview-flow.md`](references/interview-flow.md)** — 进入 Step 0.2 时读取。包含 B1-B6 规则、轮次模板、递归搜索模式、收敛检查。
- **[`references/interview-methods.md`](references/interview-methods.md)** — 需要更深入的访谈方法论时读取。行为追问、偏误检测、选项法设计。
- **[`references/benchmarking-guide.md`](references/benchmarking-guide.md)** — 进入 Phase 2 时读取。SkillHub API用法、质量排序公式、腾讯9维度比对模板。
- **[`references/meeting-action-extractor-example.md`](references/meeting-action-extractor-example.md)** — 创建 Step 2 内容时读取。一个完整的高质量Skill示例。
