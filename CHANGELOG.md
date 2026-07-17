# Changelog

All notable changes to this project will be documented in this file.

## [6.1.0] - 2026-07-17

### Changed
- 版本跳跃到 6.1.0，绕过 ClawHub 后端 6.0.0/6.0.1/6.0.2 版本号锁定
- 归集为单一开发入口：后续所有 skill-forge 修改只在 `skill-forge-v32-fixed` 目录进行
- summary 更新为"归集单一开发入口"

## [6.0.2] - 2026-07-17

### Fixed
- ClawHub 后端版本号锁定：6.0.1 publish 返回 "already exists" 但 inspect 显示 Latest=5.2.4。递增到 6.0.2 绕过锁定

## [6.0.1] - 2026-07-17

### Fixed
- **README 中英文版本号不一致**：SKILL.md/plugin.json 已是 v6.0.0 但 README.md 中英文仍停留在 v5.2.4。修复为 v6.0.1（规则 29 多文件一致性校验）
- **README 残留"技能发布"触发描述**：SKILL.md 声明"发布不在本技能范围内"，但 README 仍列"技能发布"为触发词。移除该描述，改为明确声明"技能发布不会触发本技能"（声明-行为一致性）
- **displayName 语言策略**：displayName 从纯英文 "Skill Forge" 改为双语并列 "Skill Forge 技能熔炉"（规则 33：中文 skill 应用双语并列格式）

### Changed
- 版本从 6.0.0 升级到 6.0.1（多文件一致性 + 规则 33 适配）

## [6.0.0] - 2026-07-17

### Summary
Major version bump to resolve ClawHub backend version lock issue. All v5.2.x features consolidated into a clean major release.

### Features (consolidated from v5.0.0 - v5.2.4)
- Two-entry architecture: skill-forge (forge+assess) + skill-publisher (publish independently)
- Pre-gate (Phase -1): Worth doing? Already exists? Too big?
- Five entry routes: R1 scratch / R2 dialog / R3 material / R4 draft / R5 improve
- Level adaptation: Auto-detect user level from language
- One question at a time: Each round asks only 1 question + options
- Confirmation gate: One-page summary before writing
- Step 0.4 peer pre-check: Search SkillHub before creating
- Meta-skill composition: Decompose need + pipeline orchestration
- Layered validation: Default lightweight + optional heavy
- 5 authoring principles + 10-item self-check checklist
- Declaration-behavior consistency (frontmatter = behavior)

### Changed
- Version bumped from 5.2.5 to 6.0.0 (ClawHub backend version lock resolution)

## [5.2.5] - 2026-07-17

### Fixed
- 同 [5.2.1] 的所有修复（R4 入口同类预检 + 声明-行为不一致 + summary/plugin.json 同步）
- ClawHub 后端版本号锁定：5.2.4 publish 返回 "already exists" 但 inspect 显示 Latest=5.1.0（状态不一致），递增到 5.2.5 绕过锁定

### Changed
- 版本从 5.2.4 升级到 5.2.5（ClawHub 后端版本号锁定重试）

## [5.2.4] - 2026-07-17

### Fixed
- 同 [5.2.1] 的所有修复（R4 入口同类预检 + 声明-行为不一致 + summary/plugin.json 同步）
- ClawHub 后端版本号锁定：5.2.3 publish 返回成功但 inspect not found，递增到 5.2.4 重试

### Changed
- 版本从 5.2.3 升级到 5.2.4（ClawHub 后端版本号锁定重试）

## [5.2.3] - 2026-07-17

### Fixed
- 同 [5.2.1] 的所有修复（R4 入口同类预检 + 声明-行为不一致 + summary/plugin.json 同步）
- ClawHub 后端版本号锁定：5.2.2 在 ClawHub 上 inspect not found 但 publish 报已存在（状态不一致），递增到 5.2.3 绕过锁定

### Changed
- 版本从 5.2.2 升级到 5.2.3（ClawHub 版本号锁定解决）

## [5.2.2] - 2026-07-17

### Fixed
- 同 [5.2.1] 的所有修复（R4 入口同类预检 + 声明-行为不一致 + summary/plugin.json 同步）
- SkillHub 版本号冲突：5.2.1 已被其他会话推送，递增到 5.2.2

### Changed
- 版本从 5.2.1 升级到 5.2.2（SkillHub 版本号冲突解决）

## [5.2.1] - 2026-07-17

### Fixed
- **R4 入口同类预检跳过问题**：R4（从草稿完善）原流程直接跳到 Step 4 验证，绕过了确认门和 Step 0.4 同类预检。现补全为"反推四要素→确认门→Step 0.4 同类预检→补全→验证"，确保即使用户带着成熟草稿也不跳过 SkillHub 同类搜索比对
- **R2/R3 入口同类预检指向不明确**：路由表和 references 中"→确认门"改为"→确认门→Step 0.4 同类预检"，消除走完确认门直接进 Phase 1 的歧义
- **Step 0.4 适用范围声明**：明确"所有创建类入口（R1/R2/R3/R4）必须执行，R5 改进类跳过"，避免执行时遗漏
- **声明-行为不一致（Critical）**：description 移除"技能发布"触发词和"只做GitHub+ClawHub推送"声明，入口检测表移除"技能发布"行，Phase 3 改为"发布交接提醒"，明确发布由 skill-publisher 承接
- **summary 版本信息过时**：更新到 v5.2.1，反映本次修复内容
- **plugin.json description 不一致**：同步为与 SKILL.md description 一致

### Changed
- 版本从 5.2.0 升级到 5.2.1
- Phase 3 从"发布 + 最后一公里"改为"发布交接提醒"（发布执行逻辑已移交给 skill-publisher）

## [5.2.0] - 2026-07-16

### Added
- 新增 `references/authoring-principles.md`（从 skill-auditor/skill-authoring-guide.md 反哺的 5 大原则 + frontmatter 规范 + 6 大反模式 + 10 项自检清单）
- SKILL.md frontmatter 补齐 version / license / allowed-tools / metadata.openclaw（修复与 plugin.json 的声明-行为不一致）
- References 列表新增 authoring-principles.md 索引

### Changed
- "三条铁律"扩展为引用 5 大原则（补充最小权限 + 用户知情 + 权力比例适当，原铁律2 一Skill一职并入原则 1）
- SKILL.md 格式教学示例补齐完整 frontmatter（含 metadata.openclaw 子结构）
- Step 4a Schema 检查扩展为指向 `authoring-principles.md` §六的 10 项自检清单
- 标题从 "v5.1" 更新为 "v5.2"

### Fixed
- 修复 SKILL.md frontmatter 与 plugin.json 版本号不一致（D-M3 声明-行为不一致：plugin.json 有 version=5.1.0，frontmatter 未声明 version）
- 修复 `references/publishing-guide.md` 断链引用（4.1.0 已迁移到 skill-publisher，清理 SKILL.md 正文和 References 列表中的残留引用）
- 修复 CHANGELOG 日期乱序：[5.0.0] 和 [5.1.0] 日期从 2026-06-12 修正为 2026-07-12 / 2026-07-13（与 [4.3.1] - 2026-07-11 的时序一致，5.x 应晚于 4.x）

## [5.1.0] - 2026-07-13

### Added
- **Step 0.4 同类预检**: 确认门通过后立即搜索SkillHub，避免重复造轮子。四种分支：a)有更好的→建议安装 b)有但不够好→提取差异点 c)无同类→直接创建 d)可组合→元技能组合
- **元技能组合+管线编排**: 需求分解为原子操作→逐个搜索→评估覆盖率。三种管线模式：顺序/分支/条件
- **New reference file**: composition-and-pipeline.md — 组合决策+管线编排+推荐模板

### Changed
- Phase 2 角色调整：从"SkillHub同类比对"→"质量自评+差异化验证"。同类搜索已前移到Step 0.4（创建前）
- benchmarking-guide.md 重写：从"SkillHub API比对指南"→"腾讯9维度自评+差异化验证指南"
- Step 5a-5d 重构：5a自评→5b差异化验证→5c盲区修复→5d用户决策

### Architecture
- 创建前搜索（Step 0.4）避免最大的浪费：创建完才发现已有更好的
- 创建时锚定同类竞品，设计明确的差异化优势
- 元技能组合建议：很多需求不需要新建，组合已有Skill就能解决

## [5.0.0] - 2026-07-12

### Added
- **Pre-Gate (前置闸门)**: Phase -1 added — judges "worth doing? / already exists? / too big?" before investing time.劝退 one-time tasks.
- **Five Entry Routes**: R1 从零想法 / R2 从对话提取 / R3 从现成材料 / R4 从草稿完善 / R5 改进已有skill
- **Level Adaptation (水平自适应)**: Auto-detects user level from language. Never asks "你几级". Adapts terminology depth in real-time.
- **One Question at a Time (一次一问)**: Each interview round asks only 1 question + 2-3 options. No more 3-questions-at-once.
- **Confirmation Gate (确认门)**: After 4 elements gathered, present one-page summary. "理解没对齐，绝不动手写。"
- **Layered Validation (分层验证)**: Default lightweight "跑给你看" for beginners; optional heavy 6-layer validation for power users.
- **Last Mile (最后一公里)**: Phase 3 now includes local install + self-test trigger + packaging instructions.
- **Description Optimization Iteration**: After writing initial description, test with 5 real user phrases. Auto-iterate if trigger accuracy is low. Max 3 rounds.
- **Skill Improvement Diagnosis**: Symptom → Check Point → Action diagnosis script for fixing broken skills (not triggering / running off / too verbose).
- **New reference file**: pre-gate-and-routing.md — pre-gate logic + 5 entry routes + improvement diagnosis

### Changed
- Interview flow: 2-3 questions per round → 1 question per round
- Convergence check: 5 elements → 4 elements (做什么/何时触发/输入输出/边界)
- Step 4: mandatory 6-layer → layered (default lightweight + optional heavy)
- Phase 3: publish only → publish + last mile (install + self-test + packaging)
- SKILL.md: 240 lines → 191 lines (under 200 target!)

### Removed
- Mandatory 6-layer validation for all users (replaced by layered approach)

## [4.3.1] - 2026-07-11

### Added
- **SkillHub frontmatter 字段**: 新增 slug/displayName/version/license 字段，支持 SkillHub 平台发布（与 ClawHub 的 name/description 共存于同一 frontmatter）

### Changed
- 版本从 4.3.0 升级到 4.3.1

## [4.3.0] - 2026-07-11

### Added
- **allowed-tools**: frontmatter 新增工具白名单声明（Bash(mkdir/curl) + Read/Write/Edit/Glob/Grep + WebFetch/WebSearch + AskUserQuestion），符合 TRACE T维度最小权限原则
- **示例模块**: 新增3个完整示例，覆盖常见输入（信息充足直接创建）+ 边界输入（信息不足进入访谈）+ 异常输入（需求矛盾处理），符合4模块规范和 TRACE R维度异常处理反馈要求
- **条件触发发布提醒**: 发布交接提醒从无条件触发改为条件触发，明确触发条件（验证通过≥7分 + 用户选择保持原样/修复完成）和不触发条件（直接安装已有/验证未通过/Critical盲区未修复）

### Changed
- 版本从 4.2.0 升级到 4.3.0
- 自检通过：Step 4 验证流水线自检全通过（T/R/A/C/E 五维度均达标）

## [4.2.0] - 2026-07-11

### Added
- **TRACE 评测体系整合**（方案A轻量整合）：将 SkillHub TRACE 五维度（Trust/Reliability/Adaptability/Convention/Effectiveness）的核心检查点嵌入 Step 4 验证流水线
- **Step 4a+1 信任检查（T维度）**：在原7条安全红线基础上，新增最小权限校验（检查 allowed-tools 是否有多余权限）和国内可用性校验（外部 API 必须国内可访问 + 中文交互完整性）
- **Step 4c Dogfood压力测试（R维度）**：从单一示例输入升级为三类输入（常见/边界/复杂）× 三项检查（格式匹配/规则合规/异常处理反馈），新增"异常处理反馈"检查——Skill 遇到无法完成的情况时必须清楚说明原因并给出建议，而不是返回空结果或乱码
- **Step 4e 有效性验证（E维度）**：在基线对比基础上，新增"可直接可用性"（修正量<20%为通过）和"增量价值"（信息整合/判断建议/质量提升）两层验证

### Changed
- Step 4a+1 标题从"安全红线检查"改为"信任检查（T维度）"
- Step 4c 标题从"Dogfood模拟"改为"Dogfood压力测试（R维度）"
- Step 4e 标题从"基线对比"改为"有效性验证（E维度）"

## [4.1.0] - 2026-07-11

### Changed
- 标题从"技能熔炉 v4.0"更新为"v4.1"
- Description 移除"发布"触发词，改为引导用户调用 skill-publisher
- 入口检测表移除"技能发布/发布技能 → Phase 3"行，只保留"技能熔炉"和"技能评估"两个入口
- 副标题从"锻造 → 评估 → 发布，三入口"改为"锻造 → 评估，两入口"

### Removed
- **Phase 3: 发布到 GitHub + ClawHub** 整个章节（Step 6-10）— 发布流程已由独立的 skill-publisher 技能承接
- `references/publishing-guide.md` — 该文件内容已迁移到 skill-publisher 的 references 目录

### Added
- **发布交接提醒**：Phase 2 评估完成后，主动提示用户可调用 skill-publisher 进行发布，明确声明本技能不执行任何发布操作
- 入口检测表下方新增"发布不在本技能范围内"说明，指向 skill-publisher

## [4.0.0] - 2026-06-12

### Added
- **Progressive Disclosure (渐进式披露)**: Iron Rule 3 upgraded from "150 lines" to "≤200 lines + 3-tier split (references/scripts/assets)"
- **Extended frontmatter**: allowed-tools / model / effort / metadata fields for precise tool permission and thinking depth control
- **scripts/ directory**: executable scripts for deterministic operations (checks, exports, batch processing)
- **assets/ directory**: templates, schemas, example files, output styles
- **6-layer validation pipeline**: Schema → Security (7 items) → Trigger test (5+3 real user queries) → Dogfood → Quantitative scoring (0-10) → Baseline comparison (with vs without Skill)
- **Troubleshooting module**: optional 5th module in SKILL.md format (common errors + causes + solutions)
- **Real user trigger test**: 5 positive + 3 negative real user phrasings (including colloquial, rewritten, vague expressions)

### Changed
- Iron Rule 3: "150行以内" → "渐进式披露 (≤200行 + references/scripts/assets 三级拆分)"
- SKILL.md format: 4 modules → 4+1 modules (troubleshooting optional)
- Step 4b: "3正向+3反向假问题" → "5条真实用户说法+3条反向测试"
- Step 4: added Step 4d (quantitative scoring 0-10) and Step 4e (baseline comparison)
- Tencent 9-dimension #9: "under 150 lines" → "under 200 lines + progressive disclosure"
- Directory structure: references/ only → references/ + scripts/ + assets/

### Removed
- Hard 150-line limit (replaced by 200-line + progressive disclosure)

## [3.6.0] - 2026-06-12

### Changed
- Standardized trigger words: 技能熔炉 / 技能评估(skill评估,评估技能) / 技能发布(发布技能)
- Removed redundant triggers (熔炉创建/锻造技能/新建熔炉/熔炉技能/推送技能/发布到GitHub/发布到ClawHub)
- skill-publisher trigger words aligned: 技能发布/发布技能 only

## [3.5.0] - 2026-06-12

### Added
- Three-entry trigger system: 技能熔炉(full pipeline) / 技能评估(evaluation only) / 技能发布(publishing only)
- Phase 3: Publishing to GitHub + ClawHub (from skill-publisher, now integrated)
- Entry detection logic: trigger word determines which Phase to start from
- references/publishing-guide.md: merged from skill-publisher (repo structure + security audit + publish procedures)
- skill-publisher as independent lightweight entry for "发布技能" trigger

### Changed
- Description rewritten with three trigger word groups for three scenarios
- Title changed from "技能熔炉 v3.4" to "技能熔炉 v3.5"
- Phase 2 now also serves as standalone "技能评估" entry
- SKILL.md structure: 4 phases (0→1→2→3) instead of 3 phases (0→1→2)

## [3.4.0] - 2026-06-11

### Added
- Chinese trigger words: 技能熔炉/熔炉创建/锻造技能/新建熔炉/熔炉技能 — completely separates from built-in skill-creator
- "何时触发" section with explicit trigger word list and anti-confusion rules
- Bilingual description: Chinese trigger keywords front-loaded for reliable auto-triggering

### Changed
- Title changed from "Skill Forge" to "技能熔炉" — Chinese-first branding
- Description rewritten: Chinese trigger words first, English Do NOT scope preserved
- All section headers translated to Chinese for consistency
- Version bumped from v3.3 to v3.4

## [3.3.0] - 2026-06-07

### Added
- Security Red Line Check (Step 4a+1): 7-item security scan before delivery
- Do NOT scope in description: "Do NOT use for editing existing skills, skill security vetting, or general coding tasks."

### Changed
- Refined self-validation pipeline: Schema → Security → Trigger → Dogfood
- Improved Phase 2 benchmarking flow with clearer user decision options

## [3.2.0] - 2026-06-06

### Added
- Phase 2: SkillHub Peer Benchmarking — search, rank, and compare against Top 3 peers
- Tencent 9-dimension compliance comparison template
- Quality ranking formula: downloads × 0.4 + installs × 0.3 + stars × 0.3
- Differentiation & gap analysis with Tencent Manual justification
- Progressive Disclosure: moved detailed docs to references/ directory
- benchmarking-guide.md reference

### Changed
- SKILL.md trimmed from 319 lines to ~170 lines (detailed content moved to references/)
- Interview flow extracted to references/interview-flow.md
- Interview methods extracted to references/interview-methods.md

## [3.1.0] - 2026-06-05

### Added
- Adaptive 2-5 round interview (up from fixed 3 rounds)
- B1-B6 interview rules: behavioral probing, Why×1-2, bias detection, contradiction writeback, option-first 3+1, creative option probe
- Recursive search pattern: broad → deepen → precision → verify
- 3-step self-validation: Schema check → Trigger test → Dogfood simulation
- Convergence check after each interview round
- interview-flow.md and interview-methods.md references

### Changed
- Phase 0 intent recognition: element check before deciding interview vs direct creation
- Interview rounds now adaptive based on element convergence

## [3.0.0] - 2026-06-04

### Added
- Phase 0: Intent Recognition — detect whether context is sufficient or interview is needed
- 3-round structured interview for new skill creation
- Element checklist: single scenario / trigger condition / output format / scope boundary / hard constraints

### Changed
- Restructured from flat flow to phased pipeline (Phase 0 → Phase 1)

## [2.0.0] - 2026-06-03

### Added
- Three Iron Rules: Description-first / One-Skill-One-Job / Under 150 lines
- 4-module SKILL.md format: 任务/输出格式/规则/示例
- Useless rule filter: delete "语言简洁"/"保持客观"/"排版整齐" etc.
- Intern Test for rules: if an intern can't execute it, delete it

### Changed
- Complete rewrite from v1 template-based approach to rule-driven approach

## [1.0.0] - 2026-06-01

### Added
- Initial release: basic skill creation template
- SKILL.md frontmatter with name and description
- Simple creation flow without interview or validation
