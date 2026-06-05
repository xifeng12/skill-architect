---
name: skill-architect
description: |
  设计 Agent Skill 的架构模式选择助手。专门回答"这个 skill 该用什么设计模式"和"如何组织 skill 的文件结构"。基于 5 种核心模式（Tool Wrapper, Generator, Reviewer, Inversion, Pipeline）提供架构咨询，不处理 skill 的完整创建、评估或打包流程。Triggers: "skill架构", "设计skill", "skill模式", "skill template", "skill规范", "skill design", "帮我设计skill", "skill structure", "skill pattern"。
---

# Skill Architect

专门设计 Agent Skills 的架构助手，基于 5 种核心设计模式构建符合 ADK 规范的 Skill。

## 5 种设计模式

| 模式 | 用途 | 典型场景 |
|------|------|----------|
| **Tool Wrapper** | 封装外部工具/API | 封装 yt-dlp、OCR库、REST API |
| **Generator** | 生成结构化内容 | 生成 PR Description、配置文件、代码模板 |
| **Reviewer** | 评审/检查 | 代码评审、安全审计、文档质量检查 |
| **Inversion** | 深度信息采集 | 需求分析、技术选型、项目规划 |
| **Pipeline** | 多步协调任务 | 复杂文件处理、多阶段工作流 |
| **Stateful / Memory** | 跨调用保持状态 | Any scenario requiring history, user config, or audit trail |

详细模式说明见 `references/design-patterns.md`。

## 设计流程

### Step 0: 场景分类 (Scene Classification)

Classify the user's request into one of 9 categories BEFORE selecting a Pattern. Category determines content strategy; Pattern determines file structure. These are orthogonal decisions.

| Category | Content Focus | Natural Pattern Fit |
|----------|--------------|-------------------|
| 库 / API 参考 | Correct usage + known Gotchas | Tool Wrapper |
| 产品验证 | Acceptance criteria + edge cases | Reviewer / Pipeline |
| 数据获取分析 | Data source + query format + auth | Tool Wrapper |
| 业务流程自动化 | Trigger conditions + error handling | Pipeline |
| 代码脚手架 | Template constraints + naming rules | Generator |
| 代码质量审查 | Checklist + severity levels | Reviewer |
| CI/CD 部署 | Step order + Gate conditions | Pipeline |
| Runbook | Symptom → investigation → structured report | Pipeline + Inversion |
| 基础设施运维 | Operation procedure + rollback strategy | Pipeline + Tool Wrapper |

If the request spans multiple categories, select the dominant one. Note secondary categories for the references/ structure.

Load `references/anthropic-content-quality.md` now. Apply its rules throughout Steps 1–4.

### Step 1: 需求分析 & 模式选择

分析用户需求，选择最合适的设计模式。快速决策：

```
需要调用外部工具/API？ → Tool Wrapper
需要生成结构化内容？ → Generator
需要评审/检查？ → Reviewer
需要多步骤协调？ → Pipeline
需要深度信息采集？ → Inversion
```

复杂场景可组合多种模式（如 Pipeline + Generator）。

### Step 2: 信息采集

根据选定的模式，采集关键信息：

| 模式 | 关键信息 |
|------|----------|
| Tool Wrapper | 工具名称、可用操作、参数说明、触发条件 |
| Generator | 输出格式、必需字段、模板要求 |
| Reviewer | 评审对象、检查清单、输出格式 |
| Pipeline | 步骤定义、输入输出、Gate 条件 |
| Inversion | 信息需求、Phase 定义、完成条件 |
| Stateful / Memory | State triggers (history needed? config needed?), state file type (.skill-log.json or .skill-state.json) |
| All patterns | Gotchas: what does Claude do WRONG in this domain without guidance? Collect at least 2 before proceeding. |

### Step 3: 生成 Skill 架构

使用 `assets/skill-template.md` 作为蓝图，生成：

1. **推荐目录结构**
2. **SKILL.md 内容**（含 frontmatter）
3. **references/ 文件**（如需要）
4. **assets/ 文件**（如需要）

### Step 4: 确认保存

询问用户是否需要保存到指定位置。

## Skill 目录规范

```
skill-name/
├── SKILL.md              # 必需：主配置文件
├── references/           # 可选：参考文档、规则清单
│   └── api-docs.md
└── assets/               # 可选：模板、资源文件
    └── template.md
```

### 文件职责分离

| 文件 | 内容 |
|------|------|
| `SKILL.md` | 触发条件、核心逻辑、使用说明 |
| `references/` | API 文档、规范、检查清单等参考资料 |
| `assets/` | 模板文件、示例资源 |

## 设计原则

1. **单一职责**：每个 Skill 专注一个核心功能
2. **分离关注点**：逻辑、规则、模板分开存放
3. **明确触发**：description 包含功能描述 + 触发短语
4. **保持精简**：SKILL.md 控制在 500 行以内
5. **可测试性**：设计时考虑如何验证效果
6. **内容质量**：遵循 `references/anthropic-content-quality.md` 中的 Anthropic 内容质量清单

## 与 skill-creator 的分工

| Skill | 定位 |
|-------|------|
| **skill-architect** | 架构设计：选择模式、定义结构、生成 SKILL.md 框架 |
| **skill-creator** | 创建迭代：编写内容、运行测试、评估优化 |

当需要完整创建并迭代优化 Skill 时，先由 skill-architect 设计架构，再交由 skill-creator 实现和测试。
