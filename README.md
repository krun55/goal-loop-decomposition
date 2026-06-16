# Goal Loop Decomposition Skill

## English

`goal-loop-decomposition` is an agent skill for Codex and Claude Code. It turns a broad goal into a dependency-aware loop of smaller sub-goals, keeps a controller ledger, selects the next ready sub-goal, generates execution prompts, and requires acceptance evidence before continuing.

### When to use it

Use this skill when a task is too broad to execute safely in one pass and needs:

- sub-goal decomposition
- dependency tracking
- explicit parent-goal tracking when goal-management tools are available and no incompatible active goal exists
- execution prompts for each sub-goal
- forced acceptance gates
- automatic next-sub-goal selection
- a final parent-goal verification pass

### Install

Clone this repository into your agent's skills directory.

For Codex:

```bash
git clone https://github.com/krun55/goal-loop-decomposition.git ~/.codex/skills/goal-loop-decomposition
```

For Claude Code:

```bash
git clone https://github.com/krun55/goal-loop-decomposition.git ~/.claude/skills/goal-loop-decomposition
```

Then invoke it in your agent session:

```text
Use $goal-loop-decomposition to split this goal into dependent sub-goals and run the gated loop.
```

Claude Code users can also ask naturally, for example:

```text
Use the goal-loop-decomposition skill to decompose this broad goal into gated sub-goals.
```

### What it enforces

- One controller ledger for the parent goal.
- Explicit parent-goal creation or adoption when goal-management tools are available.
- Sub-goals are accepted only with fresh verification evidence.
- Sub-goals stay in the ledger unless nested goals are explicitly supported.
- Subagents or separate threads are used only when explicitly allowed.
- Blocked sub-goals must record exact failed commands, errors, and attempted fixes.

### Claude Code notes

- `SKILL.md` is the portable skill entrypoint.
- `agents/openai.yaml` is Codex UI metadata and can be ignored by Claude Code.
- If goal-management tools are unavailable, run the loop directly from the ledger and only mark status in the ledger.
- If a non-nestable active goal already exists, adopt that active goal as the parent and keep sub-goals in the ledger.

### Repository layout

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── subgoal-ledger-template.md
    └── subgoal-prompt-template.md
```

## 中文

`goal-loop-decomposition` 是一个兼容 Codex 和 Claude Code 的 agent skill，用来把宽泛目标拆成带依赖关系的子目标循环。它要求维护 controller ledger，自动选择下一个 ready 子目标，生成可执行提示词，并且只有通过验收门后才能继续推进。

### 适用场景

当一个任务太大，不能安全地一次性执行，并且需要下面这些能力时使用：

- 拆分子目标
- 跟踪依赖关系
- 在环境支持时显式创建或接管 parent goal
- 为每个子目标生成执行提示词
- 强制验收门
- 自动选择下一个子目标
- 最后做 parent goal 总体验收

### 安装

把仓库克隆到对应 agent 的 skills 目录。

Codex：

```bash
git clone https://github.com/krun55/goal-loop-decomposition.git ~/.codex/skills/goal-loop-decomposition
```

Claude Code：

```bash
git clone https://github.com/krun55/goal-loop-decomposition.git ~/.claude/skills/goal-loop-decomposition
```

然后在 agent 会话里调用：

```text
Use $goal-loop-decomposition to split this goal into dependent sub-goals and run the gated loop.
```

Claude Code 用户也可以自然语言调用：

```text
Use the goal-loop-decomposition skill to decompose this broad goal into gated sub-goals.
```

### 核心约束

- parent goal 必须有一个 controller ledger，不能只靠上下文记忆。
- 当 goal-management 工具可用时，必须显式创建或接管 parent goal。
- 子目标必须有新鲜的验证证据，才能标记为 accepted。
- 除非环境明确支持 nested goals，否则子目标只进 ledger，不单独创建系统 goal。
- 只有在用户或工具策略明确允许时，才使用 subagent 或单独线程。
- blocked 子目标必须记录失败命令、错误、已尝试修复和阻塞原因。

### Claude Code 说明

- `SKILL.md` 是可移植的 skill 入口。
- `agents/openai.yaml` 是 Codex UI 元数据，Claude Code 可以忽略。
- 如果环境没有 goal-management 工具，就直接用 ledger loop 执行，并只在 ledger 中更新状态。
- 如果已经存在不能嵌套的 active goal，就接管这个 active goal 作为 parent，子目标仍然只写入 ledger。

### 仓库结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── subgoal-ledger-template.md
    └── subgoal-prompt-template.md
```
