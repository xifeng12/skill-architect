# SKILL.md 模板

本模板用于生成符合 ADK 规范的 Skill 配置文件。

---

## 基础结构

```markdown
---
name: {skill-name}
description: |
  {功能描述}
  {触发条件说明}
  Triggers: "{触发短语1}", "{触发短语2}"
---

# {Skill 标题}

{简短说明}

## 核心功能

{主要功能点}

## 使用方法

{使用说明}

## 注意事项

{重要提醒}
```

---

## YAML Frontmatter 规范

| 字段 | 说明 | 示例 |
|------|------|------|
| `name` | Skill 唯一标识符，小写连字符 | `skill-architect` |
| `description` | 功能描述 + 触发条件 | 见下方说明 |

### description 编写规范

```yaml
description: |
  {功能描述 - 一句话}
  {触发条件说明 - 何时使用此 skill}
  Triggers: "{短语1}", "{短语2}", "{短语3}"
```

---

## 各模式模板

### Tool Wrapper 模板

```markdown
---
name: {tool-name}-wrapper
description: |
  封装 {tool-name} 工具，提供 {核心功能} 能力。
  Triggers: "调用{工具}", "使用{工具}", "{tool-cli-cmd}"
---

# {Tool Name} Wrapper

封装 {tool-name} 的核心功能。

## 可用操作

| 操作 | 命令 | 说明 |
|------|------|------|
| {op1} | `{cmd1}` | {desc1} |

## 参数说明

见 `references/params.md`。

## 示例

{使用示例}
```

### Generator 模板

```markdown
---
name: {name}-generator
description: |
  生成 {输出类型}，遵循 {规范名称} 规范。
  Triggers: "生成{类型}", "创建{类型}", "write {type}"
---

# {Name} Generator

根据模板生成 {输出类型}。

## 必需字段

- {field1}: {说明}

## 模板

见 `assets/template.md`。

## 生成流程

1. 采集必需信息
2. 验证字段完整性
3. 填充模板
4. 输出结果
```

### Reviewer 模板

```markdown
---
name: {name}-reviewer
description: |
  评审 {评审对象}，输出结构化报告。
  Triggers: "评审{对象}", "review {object}", "检查{对象}"
---

# {Name} Reviewer

对 {评审对象} 进行评审。

## 评审准则

见 `references/checklist.md`。

## 评审流程

1. 读取目标文件
2. 逐项检查准则
3. 生成评审报告

## 输出格式

见 `assets/report-template.md`。
```

### Inversion 模板

```markdown
---
name: {name}-collector
description: |
  通过多轮访谈采集 {信息类型} 信息。
  Triggers: "分析{类型}", "采集{信息}", "{领域}分析"
---

# {Name} Collector

采集 {信息类型} 信息。

## 访谈阶段

### Phase 1: {阶段名}

**目标**: {阶段目标}
**问题**: {关键问题}
**Gate**: {完成条件}

## 禁止事项

- 信息不完整时禁止生成输出
- 禁止跳过任何 Phase
```

### Pipeline 模板

```markdown
---
name: {name}-pipeline
description: |
  执行 {任务名} 的多步流水线。
  Triggers: "{动词}{对象}", "run {name}"
---

# {Name} Pipeline

{任务描述}

## 流水线步骤

### Step 1: {步骤名}

**输入**: {输入说明}
**处理**: {处理逻辑}
**输出**: {输出说明}
**Gate**: {准入条件}

### Step 2: {步骤名}

{同上结构}
```

---

## 命名规范

- Skill 名称：小写连字符，如 `code-reviewer`
- 目录名：与 Skill 名称一致
- 文件名：小写连字符，如 `design-patterns.md`
