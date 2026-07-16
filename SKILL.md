---
name: "skill-forge"
description: "技能熔炉 — 锻造/评估/发布 Skill。说 技能熔炉 走全流程；说 技能评估/skill评估/评估技能 只做同类比对+腾讯9维度；说 技能发布/发布技能 只做GitHub+ClawHub推送。Do NOT use for editing existing skills, skill security vetting, or general coding tasks."
---

# 技能熔炉 v5.1

锻造 → 评估 → 发布，三入口全流程交付可自动触发、稳定输出的 Skill。

## 入口检测

| 触发词 | 入口 | 执行流程 |
|--------|------|---------|
| 技能熔炉 | Phase -1 | 前置闸门→入口路由→访谈→确认门→同类预检→创建→验证→评估→发布 |
| 技能评估 / skill评估 / 评估技能 | Phase 2 | 只做 SkillHub 同类比对 + 腾讯9维度 |
| 技能发布 / 发布技能 | Phase 3 | 只做 GitHub + ClawHub 推送 |

**检测到触发词后，立即跳转到对应 Phase，不执行前面的阶段。**

## 三条铁律

**铁律1：Description先行** — AI每轮对话扫描所有Skill的description，模糊=永远不触发=死Skill。

**铁律2：一Skill一职** — 多功能Skill触发混乱、输出不一致。

**铁律3：渐进式披露** — SKILL.md ≤200行，只放导航。详细内容拆入 `references/` + `scripts/` + `assets/`。

## SKILL.md 格式

```markdown
---
name: "<skill-name>"
description: "<做什么 + 何时触发。核心关键词放前200字符>"
allowed-tools: "<工具白名单>"
model: "<推荐模型>"
effort: "<low/medium/high>"
metadata: { author, version, category }
---

# <技能标题>
## 任务
## 输出格式
## 规则
## 示例
## 故障排除（可选）
```

## 目录结构

```
<skill-name>/
├── SKILL.md          # 主入口（≤200行）
├── references/       # 长文档、方法论、详细案例
├── scripts/          # 可执行脚本（确定性操作）
├── assets/           # 模板、schema、示例文件
├── README.md         # 中英双语说明
├── CHANGELOG.md      # 版本日志
├── LICENSE           # MIT-0
└── .claude-plugin/plugin.json
```

---

## Phase -1: 前置闸门

**【入口：技能熔炉】** — 读取 [`references/pre-gate-and-routing.md`](references/pre-gate-and-routing.md) 获取完整闸门+路由方法论。

**动手前先判断三件事，该劝退就劝退：**

| 检查 | 通过 | 劝退 |
|------|------|------|
| 值不值得做？最近一周≥3次？做法固定？输出可预期？ | ≥2个Yes → 继续 | 一次性任务→"直接问AI更快" |
| 有没有现成的？SkillHub上有同类吗？ | 没有 or 有差距 → 继续 | 有且很好→"建议安装: skillhub install <slug>" |
| 是不是太大了？该拆成几个？ | 单一场景 → 继续 | 多场景→"建议拆开，先做哪个？" |

## Phase 0: 入口路由与需求共创

**【入口：技能熔炉】** — 读取 [`references/pre-gate-and-routing.md`](references/pre-gate-and-routing.md) Part 2-3 + [`references/interview-flow.md`](references/interview-flow.md) 获取完整方法论。

### Step 0.1: 五类入口路由

| 入口 | 信号 | 策略 |
|------|------|------|
| R1 从零想法 | "我想做个skill" | 自适应访谈（Step 0.2） |
| R2 从对话提取 | "把刚才对话变成skill" | 扫描上下文→提取步骤→生成草稿→确认门 |
| R3 从现成材料 | 给文档/SOP | 分析材料→反推四要素→补缺→确认门 |
| R4 从草稿完善 | 给半成品SKILL.md | 检查缺失模块→补全→验证 |
| R5 改进已有skill | "不触发/跑偏/太啰嗦" | 诊断：症状→检查点→动作→修复→验证 |

### Step 0.2: 自适应访谈（2-5轮，一次一问）

**水平自适应**：从用户措辞判断水平。张口pandas→用术语；说"差不多就行"→换大白话。**全程不问"你几级"**。

**一次一问**：每轮只问1个问题+2-3个选项。一次甩3个问题，用户只会挑最好答的。

**四要素**：做什么 / 何时触发 / 输入输出 / 边界。≥3个明确→进入确认门。

### Step 0.3: 确认门

**理解没对齐，绝不动手写。**

```
我理解是这样——
· 做什么：[一句话]
· 何时触发：[用户会说的话]
· 输入：[格式]；输出：[格式]
· 边界：[不做什么]
这样对吗？没问题我就开始写了。
```

用户确认 → Step 0.4 同类预检。用户纠正 → 修正后重新确认。

### Step 0.4: 同类预检（创建前）

**【入口：技能熔炉】** — 读取 [`references/composition-and-pipeline.md`](references/composition-and-pipeline.md) 获取组合与管线编排方法论。

**确认门通过后，立即搜索 SkillHub，避免重复造轮子：**

| 分支 | 条件 | 动作 |
|------|------|------|
| **a) 有现成的更好** | 找到高质量同类(≥7分) | 建议安装已有Skill，结束流程 |
| **b) 有但不够好** | 有同类但有明显差距 | 提取差异点→作为Phase 1设计输入 |
| **c) 无同类** | 没有同类Skill | 直接进入Phase 1创建 |
| **d) 可组合** | 需求可分解为多个原子操作 | 元技能组合+管线编排建议 |

**分支d详解**：需求分解为原子操作→逐个搜索→评估覆盖率：
- 全组合：所有步骤都有高质量Skill→建议安装+编排管线，无需新建
- 部分组合：部分有高质量→安装已有的+只新建缺失的
- 全新建：无高质量同类→直接Phase 1

---

## Phase 1: 创建

**【入口：技能熔炉】**

### Step 1: Description先行 + 触发优化迭代

**格式**: `"<做什么>. 当用户说<触发词>时触发. Do NOT use for <排除范围>."`

**触发优化迭代**：初版description写完后，用5条真实用户说法测试触发准确率。触发不准→自动迭代用词。最多3轮。

### Step 2: 撰写4+1模块

任务（锁定边界）/ 输出格式（固定结构）/ 规则（3-5条，实习生测试）/ 示例（完整输入输出）/ 故障排除（可选）

### Step 3: 创建目录和文件

有确定性操作→创建 `scripts/`。有模板样式→创建 `assets/`。

### Step 4: 分层验证

**默认轻量验证（小白/日常）：**
- **Step 4a**: Schema检查（8项✅）
- **Step 4b**: 安全红线（7条RED FLAG）
- **Step 4c**: 跑给你看 — 拿真实输入跑一遍→看结果→确认/微调

**可选重型验证（老手/严谨场景）：**
- **Step 4d**: 触发测试 — 5条真实用户说法+3条反向
- **Step 4e**: 量化评分（0-10）
- **Step 4f**: 基线对比（有Skill vs 无Skill）

**最多3次迭代。3次后建议"先发布V1再迭代"。**

---

## Phase 2: 质量自评 + 差异化验证

**【入口：技能评估 / skill评估 / 评估技能】** — 读取 [`references/benchmarking-guide.md`](references/benchmarking-guide.md)。

> **角色调整**：同类搜索已前移到 Step 0.4（创建前）。Phase 2 现在聚焦于创建后的质量自评和差异化验证。

### Step 5a: 腾讯9维度自评 — 触发精准度/关键词前置/Do NOT/单一职责/4模块/输出具体性/实习生测试/示例覆盖/体积控制。逐维度自评，标出弱项。

### Step 5b: 差异化验证 — 如果 Step 0.4 发现有同类，验证差异化优势是否落地。如果无同类，跳过。

### Step 5c: 盲区修复 — 列出弱项和盲区，附腾讯手册依据，提出修复方案。

### Step 5d: 用户决策 — 采纳修复 / 保持原样。**用户决策为最终决策。**

---

## Phase 3: 发布 + 最后一公里

**【入口：技能发布 / 发布技能】** — 读取 [`references/publishing-guide.md`](references/publishing-guide.md)。

### Step 6: 仓库结构 — SKILL.md / README.md(双语) / CHANGELOG.md / LICENSE(MIT-0) / .gitignore / plugin.json

### Step 7: 安全审查 — 凭证泄露/本地路径/危险命令三类扫描，全部PASS才继续。

### Step 8: GitHub推送 — 优先git push，失败降级为REST API。创建Release。

### Step 9: ClawHub发布 — `clawhub publish --tags "<ASCII-only>"`（中文报错！）

### Step 10: 最后一公里 — 告诉用户：
1. Skill放在哪个目录
2. 怎么确认真的装上了
3. 用真实说法自测会不会被触发
4. 要发给别人怎么打包

---

## References

- **[pre-gate-and-routing.md](references/pre-gate-and-routing.md)** — Phase -1 闸门 + 五类入口路由 + 改进诊断脚本
- **[interview-flow.md](references/interview-flow.md)** — 一次一问 + 水平自适应 + 确认门 + B1-B6规则
- **[composition-and-pipeline.md](references/composition-and-pipeline.md)** — Step 0.4 元技能组合 + 管线编排方法论
- **[interview-methods.md](references/interview-methods.md)** — 行为追问、偏误检测、选项法深度参考
- **[benchmarking-guide.md](references/benchmarking-guide.md)** — 腾讯9维度自评模板 + 差异化验证
- **[publishing-guide.md](references/publishing-guide.md)** — 仓库结构 + 安全审查 + GitHub API降级 + ClawHub CLI
- **[meeting-action-extractor-example.md](references/meeting-action-extractor-example.md)** — 完整Skill示例
