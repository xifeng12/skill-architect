# ADK Skill Design Patterns

5 种核心设计模式，用于构建高质量 Agent Skills。

---

## 1. Tool Wrapper（工具封装模式）

**用途**：封装外部库、CLI 工具或 API，使其对 Agent 可用。

```
skill-name/
├── SKILL.md           # 调用规则、参数说明、触发条件
├── references/
│   └── api-docs.md    # API 文档摘要、参数说明
└── assets/
    └── examples/      # 示例文件（可选）
```

**设计要点**：
- `SKILL.md` 只写"怎么调用"，不写"API 细节"
- API 文档、参数表放在 `references/`
- 提供明确的触发短语（triggers）

**示例**：封装 yt-dlp、paddleocr、REST API

---

## 2. Generator（生成器模式）

**用途**：根据模板生成结构化输出（代码、文档、配置文件等）。

```
skill-name/
├── SKILL.md           # 生成流程、字段要求
├── references/
│   └── conventions.md # 编码规范、命名约定
└── assets/
    └── template.md    # 输出模板
```

**设计要点**：
- 使用 `assets/` 存放模板文件
- `SKILL.md` 定义必需字段和生成逻辑
- 分离"模板"与"填充规则"

**示例**：生成 PR Description、README.md、配置文件

---

## 3. Reviewer（评审模式）

**用途**：对代码、文档、配置等进行评审，输出结构化评审报告。

```
skill-name/
├── SKILL.md           # 评审流程、输出格式
├── references/
│   └── checklist.md   # 评审准则清单
└── assets/
    └── report-template.md  # 评审报告模板
```

**设计要点**：
- **关键**：分离 Checklist 与评审协议
- Checklist 放在 `references/`，可独立更新
- 评审流程写在 `SKILL.md`

**示例**：代码评审、安全审计、文档质量检查

---

## 4. Inversion（反转模式）

**用途**：通过多轮访谈采集信息，确保输入完整后再行动。

```
skill-name/
├── SKILL.md           # 访谈流程、Phase 定义
├── references/
│   └── questions.md   # 问题库（可选）
└── assets/
    └── form.md        # 信息采集表（可选）
```

**设计要点**：
- **Phase-based**：定义清晰的访谈阶段
- **禁止提前行动**：未完成采集前不生成输出
- **Gate 条件**：每个 Phase 有明确的完成条件

**示例**：需求分析、项目规划、技术选型咨询

---

## 5. Pipeline（流水线模式）

**用途**：协调多步骤任务，确保每个步骤按条件执行。

```
skill-name/
├── SKILL.md           # Step 定义、Gate 条件、流转规则
├── references/
│   └── rules.md       # 业务规则（可选）
└── assets/
    └── templates/     # 各步骤的模板（可选）
```

**设计要点**：
- **显式 Step**：每个步骤有明确输入输出
- **Gate 条件**：定义步骤间的准入条件
- **状态追踪**：记录当前执行状态

**示例**：复杂文件处理、多阶段代码生成、端到端工作流

---

## 模式组合

复杂 Skill 可以组合多种模式：

| 组合 | 适用场景 |
|------|----------|
| Pipeline + Inversion | 需要信息采集的多步任务 |
| Pipeline + Generator | 多阶段内容生成 |
| Inversion + Reviewer | 带信息核对的评审 |
| Tool Wrapper + Pipeline | 多工具协调工作流 |

---

## 最佳实践

1. **单一职责**：每个 Skill 专注一个核心功能
2. **分离关注点**：逻辑、规则、模板分开存放
3. **明确触发**：提供清晰的 triggers 描述
4. **可测试性**：设计时可考虑如何验证效果
5. **保持精简**：SKILL.md 控制在 500 行以内

---

## Content Strategy by Scene Category

These content guidelines apply AFTER pattern selection. Each category has distinct content priorities derived from Anthropic's production skill lessons.

### 1. 库 / API 参考
- Lead with Gotchas: what does Claude do wrong when calling this API without guidance?
- Include auth patterns, rate limit handling, and deprecated parameter warnings
- Do NOT restate what the API docs already say

### 2. 产品验证
- Define acceptance criteria precisely — not "works correctly" but specific pass/fail conditions
- Include edge cases that caused failures in past evals
- This category has the highest quality uplift potential

### 3. 数据获取分析
- Document data source locations, query formats, and authentication requirements
- Include rate limits and pagination patterns
- Specify error handling for network failures and data format mismatches

### 4. 业务流程自动化
- Describe trigger conditions and termination conditions explicitly
- Include error handling and rollback paths
- Avoid "railway" instructions — give Claude flexibility for variations

### 5. 代码脚手架
- Define template constraints and naming rules precisely
- Include file structure requirements and mandatory sections
- Specify what must be generated vs what is optional

### 6. 代码质量审查
- Provide checklist with severity levels (critical/warning/info)
- Include specific patterns that indicate bugs or security issues
- Define output format for review reports

### 7. CI/CD 部署
- Define step order with explicit gate conditions between stages
- Include rollback triggers and failure handling
- Specify environment-specific configurations

### 8. Runbook
- Structure: Symptom → Investigation steps → Structured output format
- Include which tools to call in what order
- Output must be structured (JSON or defined Markdown sections) for downstream processing

### 9. 基础设施运维
- Document operation procedures with explicit rollback strategies
- Include pre-flight checks and post-operation validation
- Specify logging and audit trail requirements

### All Categories
- Apply `references/anthropic-content-quality.md` checklist before finalizing SKILL.md
