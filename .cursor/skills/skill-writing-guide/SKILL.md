---
name: skill-writing-guide
description: 教 Agent 如何设计与撰写 Claude/Cursor 的 Agent Skills。当用户要「写一个 skill」「创建 skill」「设计 skill 文档」「改进 skill」「参照 Anthropic 规范写 skill」「skill 的 frontmatter 怎么写」「skill 不触发怎么办」时使用。基于 Anthropic《The Complete Guide to Building Skills for Claude》与 skill-creator 设计流程，覆盖需求捕获、草稿、测试、评审、改进的完整循环。
---

# 写 Skill 的 Skill（Skill 创作指南）

本 skill 整合 [Anthropic 官方《The Complete Guide to Building Skills for Claude》](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf) 与可选的 skill-creator 设计流程，供你在写**任何** skill 时参照。

---

## 一、核心循环（与官方指南对应）

```
需求 → 草稿 → 测试 → 评审 → 改进 → 重复
```

| 阶段 | 官方指南对应 | 要点 |
|------|--------------|------|
| **需求** | Planning and design：从 use case 出发 | 2～3 个具体用例，谁在什么情境下要什么结果 |
| **草稿** | Fundamentals + Writing：SKILL.md 结构、YAML、指令 | frontmatter 决定是否被加载，正文要具体可执行 |
| **测试** | Testing and iteration：触发测试 + 功能测试 + 对比 baseline | 相关 query 能触发、不相关不触发、输出正确 |
| **评审** | Define success criteria + Patterns | 量化/定性标准、是否跟其他 skill 兼容 |
| **改进** | Troubleshooting：under/over-trigger、执行失败 | 改 description 或指令，迭代 |

---

## 二、官方规范精华摘要

### 2.1 Skill 是什么（文件结构）

- **必须**：`SKILL.md`（Markdown + YAML frontmatter）
- **可选**：`scripts/`、`references/`、`assets/`
- **禁止**：在 skill 文件夹内放 `README.md`（文档一律放在 SKILL.md 或 references/）

### 2.2 渐进式披露（Progressive Disclosure）

- **第一层**：YAML frontmatter — 始终在 system 里，只给「做什么 + 何时用」，控制何时加载 skill。
- **第二层**：SKILL.md 正文 — 在判定相关后加载，写完整步骤与规范。
- **第三层**：references/ 等链接文件 — 按需让 Agent 打开，避免一次塞满上下文。

### 2.3 YAML frontmatter（最重要）

- **name**（必填）：kebab-case，与文件夹名一致，无空格、无大写。
- **description**（必填）：**必须同时包含**「做什么」和「何时用」（触发条件）；可包含用户常说的短语、涉及的文件类型；≤1024 字符；**禁止** XML 尖括号 `<>`。
- **可选**：license、compatibility、metadata（如 author、version、mcp-server）。

**description 好坏的例子（来自官方）：**

- 好：具体 + 可行动 + 含触发语 — *"Analyzes Figma design files and generates developer handoff documentation. Use when user uploads .fig files, asks for 'design specs', 'component documentation', or 'design-to-code handoff'."*
- 差：太泛 — *"Helps with projects."*；缺触发 — *"Creates sophisticated documentation."*；只有技术无用户话术 — *"Implements the Project entity model."*

### 2.4 正文指令（Instructions）建议

- **结构**：Steps（步骤） + Examples（示例） + Troubleshooting（常见错误与解决）。
- **具体可执行**：用「运行某命令」「先读 references/xx.md」等，避免「请合理验证」这类模糊表述。
- **错误处理**：写明常见失败原因与修复方式（如 MCP 未连接、参数错误）。
- **引用资源**：用「见 references/xxx」而不是把长文全部塞进 SKILL.md。

### 2.5 三类常见 Skill 用例（官方）

1. **Document & Asset Creation**：产出一致的高质量文档/设计/代码，常带 style guide、模板、检查清单。
2. **Workflow Automation**：多步骤流程，有固定顺序与校验，可配合 skill-creator 式步骤。
3. **MCP Enhancement**：在已有 MCP 工具之上加「怎么用」的工作流与领域知识。

### 2.6 成功标准（可衡量）

- **触发**：相关 query 约 90% 会加载该 skill；不相关不加载。
- **执行**：工作流在有限步内完成、API/MCP 调用不失败。
- **体验**：用户不需要反复提醒「下一步」、结果一致可复现。

### 2.7 排错速查（Troubleshooting）

- **Skill 不触发**：description 太泛或缺少用户会说的触发语 → 补上「Use when user says / asks for …」。
- **触发太频繁**：加负面触发（Do NOT use for …）、收窄范围、写清边界。
- **加载了但不按指令做**：指令太长或含糊 → 精简、分步骤、关键处用「CRITICAL: …」；必要时用脚本做校验（代码比自然语言确定）。
- **上传报错**：检查文件名必须是 `SKILL.md`（大小写）、YAML 用 `---` 包住、name 为 kebab-case、无 `<>`。

---

## 三、详细步骤（Skill-Creator 设计流程）

以下流程可与「核心循环」配合使用；若环境无 subagent/grader，可只做 1～2 与 7～8。

### 1. 捕获意图

- Skill **要做什么**？（一句话 + 2～3 个 use case）
- **什么时候触发**？（用户会怎么说、会传什么文件/链接？）
- **期望输出是什么**？（文档、代码、配置、还是多步操作结果？）
- 是否需要**测试用例**？（若要做 evals，先列 2～3 个真实场景）

### 2. 写草稿

- 创建 `SKILL.md`：先写 **frontmatter**（name、description），再写**指令**。
- 保持简洁（建议正文 <500 行）；把细节放到 `references/` 并链接。
- 多写「为什么」（例如：为什么要先拉取再解析），少写硬性「MUST/NEVER」条条，便于 Agent 理解意图。

### 3. 创建测试用例

- 2～3 个真实用户场景（应触发 + 期望行为）。
- 若有 evals 基础设施：保存到 `evals/evals.json`，可先不写断言（assertions）。

### 4. 并行运行测试（可选）

- 每个场景跑两路：`with_skill` / `without_skill`（baseline），对比输出与步数。
- 输出保存到 workspace，供后续断言与评审使用。

### 5. 起草断言（可选）

- 在测试运行时同步写**客观可验证**的检查点（如「输出中包含某关键字」「调用了某工具」）。
- 更新 `eval_metadata.json` 等配置。

### 6. 评分和聚合（可选）

- Grader agent 对每个断言打分，聚合成 `benchmark.json`，生成对比报告。

### 7. 启动评审界面（可选）

- 例如：`python eval-viewer/generate_review.py`。
- **Outputs**：逐个看测试结果；**Benchmark**：量化对比；收集用户反馈。

### 8. 读取反馈并改进

- 读 `feedback.json`（或等价反馈），针对「不触发 / 误触发 / 执行错」修改 description 或指令。
- 进入下一轮迭代（回到 2 或 3）。

---

## 四、关键原则（与官方一致）

1. **渐进式披露**：metadata → SKILL.md 正文 → 资源文件，按需加载。
2. **解释原因**：多用「因为…所以先…」而非单纯 MUST/NEVER，便于泛化。
3. **泛化而非过拟合**：从少量测试推广到通用场景，避免只对几句固定话术有效。
4. **保持精简**：删掉不起作用或重复的说明，控制 token 与阅读负担。
5. **可组合**：假设会与其他 skill 同时加载，避免假设「只有我一个 skill」。

---

## 五、可选：Description 优化

若具备自动化能力，可用 `run_loop.py` 等对 **description** 做 A/B 或迭代优化，提高「该触发时触发、不该触发时不触发」的准确率。优化时仍须遵守：含「做什么」+「何时用」+ 无 `<>`、name 合规。

---

## 六、写新 Skill 时的自检清单（基于官方 Quick checklist）

**动笔前**：是否已有 2～3 个具体 use case？工具/依赖是否明确（内置能力 or MCP）？

**写作中**：文件夹 kebab-case？`SKILL.md` 拼写正确？YAML 有 `---`？name 为 kebab-case？description 是否同时包含「做什么」和「何时用」？是否有示例与 Troubleshooting？

**发布前**：是否在「应触发」和「不应触发」的句子上都试过？输出是否稳定、符合预期？

---

## 七、参考链接

- [Anthropic: The Complete Guide to Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)
- [Agent Skills 开放标准](https://github.com/anthropics/skills)（与 MCP 类似，可跨平台复用）
- 官方示例技能库：anthropics/skills，可克隆后按需改写成自己的 skill。
