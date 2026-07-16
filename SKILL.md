---
name: "skill-forge"
description: "技能熔炉 — 锻造/评估/发布 Skill。说 技能熔炉 走全流程；说 技能评估/skill评估/评估技能 只做同类比对+腾讯9维度；说 技能发布/发布技能 只做GitHub+ClawHub推送。Do NOT use for editing existing skills, skill security vetting, or general coding tasks."
---

# 技能熔炉 v5.0

锻造 → 评估 → 发布，三入口全流程交付可自动触发、稳定输出的 Skill。

## 入口检测

| 触发词 | 入口 | 执行流程 |
|--------|------|---------|
| 技能熔炉 | Phase -1 | 前置闸门→入口路由→访谈→确认门→创建→验证→比对→发布 |
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

用户确认 → Phase 1。用户纠正 → 修正后重新确认。

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

## Phase 2: SkillHub 同类比对 + 腾讯9维度评估

**【入口：技能评估 / skill评估 / 评估技能】** — 读取 [`references/benchmarking-guide.md`](references/benchmarking-guide.md)。

### Step 5a: 搜索排名 — SkillHub API，`downloads × 0.4 + installs × 0.3 + stars × 0.3`，取Top 3。

### Step 5b: 腾讯9维度比对 — 触发精准度/关键词前置/Do NOT/单一职责/4模块/输出具体性/实习生测试/示例覆盖/体积控制。

### Step 5c: 差异化与盲区 — 重复→建议安装已有；有差异→记录；有盲区→列出修复。

### Step 5d: 用户决策 — 采纳修复 / 保持原样 / 安装已有。**用户决策为最终决策。**

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
- **[interview-methods.md](references/interview-methods.md)** — 行为追问、偏误检测、选项法深度参考
- **[benchmarking-guide.md](references/benchmarking-guide.md)** — SkillHub API + 质量排序 + 腾讯9维度模板
- **[publishing-guide.md](references/publishing-guide.md)** — 仓库结构 + 安全审查 + GitHub API降级 + ClawHub CLI
- **[meeting-action-extractor-example.md](references/meeting-action-extractor-example.md)** — 完整Skill示例
