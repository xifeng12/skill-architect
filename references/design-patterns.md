# ADK Skill Design Patterns

本仓库保留 **5 个 Pattern 名称**：Tool Wrapper、Generator、Reviewer、Inversion、Pipeline。

这 5 个名称用于描述确实匹配的 Skill 架构行为；**不是每个 Skill 都必须选择一个 Pattern**。如果没有任何一个能实质解释 Skill 的核心可复用行为，使用：

```text
Pattern = none
```

`Stateful/Memory`、`Reference-heavy`、coordination style、runtime capability policy 等属于局部行为、内容策略或实现特征，不新增为 Pattern 名称。

---

## 1. Tool Wrapper（工具封装模式）

**用途**：当 Skill 的核心可复用价值是让 Agent 正确使用外部库、CLI、API、SDK、MCP 或其他工具能力。

```text
skill-name/
├── SKILL.md           # 触发、调用边界、核心使用规则
├── references/
│   └── api-docs.md    # 按需加载的 API/参数/错误信息
└── scripts/           # 仅当确定性包装/校验确有价值时添加
```

**设计要点**：
- 根文件写调用语义、边界、关键 gotcha，不复制整份 API 文档；
- 参数表和长说明放 `references/`；
- 工具缺失不自动等于“安装它”；能力发现和环境修改必须分阶段。

---

## 2. Generator（生成器模式）

**用途**：根据约束或模板生成结构化输出（代码、文档、配置、模板实例等）。

```text
skill-name/
├── SKILL.md
├── references/
│   └── conventions.md
└── assets/
    └── template.md
```

**设计要点**：
- 使用 `assets/` 存放真正需要复用的模板；
- `SKILL.md` 定义生成约束、必需字段和行为边界；
- 分离模板本体与填充/判断规则。

---

## 3. Reviewer（评审模式）

**用途**：对代码、文档、配置、方案或结果进行评审、核验、审计或判定。

```text
skill-name/
├── SKILL.md
├── references/
│   └── checklist.md
└── assets/
    └── report-template.md  # 仅当稳定格式确有价值
```

**设计要点**：
- 分离评审协议与详细 checklist；
- 评审准则可放 references 并独立迭代；
- 输出格式应服务实际消费方，不为了结构化而结构化。

---

## 4. Inversion（反转模式）

**用途**：任务在行动前必须获取一组缺失信息，否则建议或执行容易失真。

```text
skill-name/
├── SKILL.md
├── references/
│   └── questions.md   # 可选
└── assets/
    └── form.md        # 可选
```

**设计要点**：
- 明确哪些信息缺失会阻止下一步；
- Gate 应服务真实决策，不机械要求填完所有字段；
- 用户/环境已提供足够信息时，不重复提问。

---

## 5. Pipeline（流水线模式）

**用途**：Skill 的核心可复用行为是协调多个阶段、Gate 或产物，并且顺序/阶段边界会影响正确性。

```text
skill-name/
├── SKILL.md
├── references/
│   └── workflow.md
└── scripts/
    └── validate.*     # 仅当确定性验证确有价值
```

**设计要点**：
- 阶段存在应有任务语义依据，而不是把简单查询人为切成流程；
- 明确跨阶段条件和 STOP；
- 后续阶段不是“存在就必须执行”，例如 discovery 完成后未必需要 reader/action；
- 不把可变研究任务写成僵硬 railway。

---

## Pattern Composition

复杂 Skill 可以真实组合多个上述 Pattern，但 **Hybrid 不是新的 Pattern 名称**。

示例：

| Composition | 适用场景 |
|---|---|
| Pipeline + Inversion | 多阶段任务中，前置阶段必须收集关键缺失信息 |
| Pipeline + Generator | 多阶段结构化内容生成 |
| Inversion + Reviewer | 评审前必须先补齐关键上下文 |
| Tool Wrapper + Pipeline | 多阶段流程中正确工具调用本身也是核心能力 |

如果一个 Pattern 明显主导，可以说明 dominant Pattern；如果没有必要，不强制排序。

---

## Pattern 之外的架构维度

这些因素可能非常重要，但不应改名为新 Pattern：

- **Reference-heavy**：大部分细节按需从 references 加载，是内容策略；
- **Stateful / Memory**：需要历史、配置、审计或跨运行连续性，是状态需求；
- **Coordination**：并行、顺序、handoff、synthesis 等应按实际行为描述，不强制闭合枚举；
- **Runtime capability policy**：工具是否可用、语义覆盖是否等价、fallback/STOP 如何处理，是运行时策略。

`none` 对这些局部行为同样有效：不要为了模板完整性添加不需要的状态、reference、协调或 provider 管理。

---

## Runtime-informed lessons

项目级 `external-knowledge v0.2.1` 验证支持了以下通用设计方向：

1. semantic trigger 可以基于意图而不是字面词；
2. `operational_status` 与 semantic `coverage` 要分离；
3. `UNKNOWN` 不等于 `MISSING`；
4. degraded fallback 可以在披露覆盖损失后完成任务；
5. capability 缺失/未验证不自动授权安装或修环境；
6. 不同阶段（如 discovery / reader）可以有独立 STOP；
7. 不要为了完整性 fan-out；
8. 工具名不同不代表 failure domain 独立。

这些是项目 runtime evidence，不是 Pattern 定义。详见 `runtime-evidence-external-knowledge-v0.2.1.md`。

---

## Content Strategy by Scene Category

这些内容策略在 Pattern（或 `none`）判断之后使用。

### 1. 库 / API 参考
- Lead with Gotchas: 没有 Skill 时模型最容易犯什么具体错误？
- 包含必要的 auth、版本、弃用参数和运行时边界；
- 不重复已有文档能直接查询到的大段静态知识。

### 2. 产品验证
- 明确定义 acceptance criteria；
- 优先沉淀真实 eval 暴露的 edge case；
- 将“问题真实存在”和“写进 Skill 后是否改善模型”分开验证。

### 3. 数据获取分析
- 记录 source semantics、query/reader 边界和证据要求；
- 工具可用性与语义覆盖分开；
- fallback 必须有具体 gap 或 preferred path 非 viable 的证据；
- 足够回答时 STOP。

### 4. 业务流程自动化
- 描述真实 trigger、阶段和终止条件；
- 包含必要错误处理和 rollback；
- 避免 railway instructions，保留对合理变化的适应性。

### 5. 代码脚手架
- 定义模板约束和命名规则；
- 明确必需/可选产物；
- 模板放 assets，判断规则留在 Skill。

### 6. 代码质量审查
- Checklist 需要具体、可判定；
- 严重性只有在消费方确实需要时才引入；
- 评审结果格式与后续使用匹配。

### 7. CI/CD 部署
- 明确 Gate、失败触发和 rollback；
- 环境假设必须显式；
- validation 与 implementation 不得静默跨阶段。

### 8. Runbook / diagnostics
- 从症状和证据出发，不预设唯一根因；
- 新动作必须能减少具体未知；
- 证据足够后停止扩展排查。

### 9. 基础设施运维
- 记录 pre-flight、blast radius、rollback 和 post-check；
- capability gap 不等价于修复授权；
- 高风险环境变化需要单独授权边界。

### All Categories
- Apply `references/anthropic-content-quality.md` when relevant;
- do not force a Pattern, script, asset, state file, hook, or reference merely to complete a template.
