# Goal Loop Decomposition Skill

## English

`goal-loop-decomposition` is a Codex skill for turning a broad goal into a dependency-aware loop of smaller sub-goals. It keeps a controller ledger, selects the next ready sub-goal, generates execution prompts, and requires acceptance evidence before continuing.

### When to use it

Use this skill when a task is too broad to execute safely in one pass and needs:

- sub-goal decomposition
- dependency tracking
- goal-mode execution prompts
- forced acceptance gates
- automatic next-sub-goal selection
- a final parent-goal verification pass

### Install

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/krun55/goal-loop-decomposition.git ~/.codex/skills/goal-loop-decomposition
```

Then invoke it in Codex:

```text
Use $goal-loop-decomposition to split this goal into dependent sub-goals and run the gated loop.
```

### What it enforces

- One controller ledger for the parent goal.
- Sub-goals are accepted only with fresh verification evidence.
- System goal tools are treated as single-active-goal unless the environment says otherwise.
- Subagents or separate threads are used only when explicitly allowed.
- Blocked sub-goals must record exact failed commands, errors, and attempted fixes.

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

`goal-loop-decomposition` 是一个 Codex skill，用来把宽泛目标拆成带依赖关系的子目标循环。它要求维护 controller ledger，自动选择下一个 ready 子目标，生成可执行提示词，并且只有通过验收门后才能继续推进。

### 适用场景

当一个任务太大，不能安全地一次性执行，并且需要下面这些能力时使用：

- 拆分子目标
- 跟踪依赖关系
- 生成 goal mode 执行提示词
- 强制验收门
- 自动选择下一个子目标
- 最后做 parent goal 总体验收

### 安装

把仓库克隆到 Codex skills 目录：

```bash
git clone https://github.com/krun55/goal-loop-decomposition.git ~/.codex/skills/goal-loop-decomposition
```

然后在 Codex 中调用：

```text
Use $goal-loop-decomposition to split this goal into dependent sub-goals and run the gated loop.
```

### 核心约束

- parent goal 必须有一个 controller ledger，不能只靠上下文记忆。
- 子目标必须有新鲜的验证证据，才能标记为 accepted。
- 除非环境明确支持，否则系统 goal 工具按单 active goal 处理。
- 只有在用户或工具策略明确允许时，才使用 subagent 或单独线程。
- blocked 子目标必须记录失败命令、错误、已尝试修复和阻塞原因。

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
