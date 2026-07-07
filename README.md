# Task Audit Skill

`task-audit` 是一个 Codex 收尾审计技能，用来在复杂任务结束前暴露不确定性、遗漏、风险和下一步补救动作。

它不做普通总结，而是回答两个问题：

- Agent 最没把握的地方是什么？
- 用户可能漏掉、但会影响验收或后续推进的问题是什么？

## 适合场景

- 复杂开发任务结束前
- 多轮调研、排障或部署收尾时
- 跨仓库、跨工具、跨系统的任务完成后
- 有测试未跑、验证失败、事实依赖推断或需求边界不清时
- 用户想问“还有什么隐患”“你最没把握什么”“我漏了什么”时

不适合一次性简单问答、格式调整或单条命令结果说明。

## 使用方式

在 Codex 中显式调用：

```text
用 $task-audit 收尾：你最没把握什么？我漏了什么？
```

也可以说：

```text
最终回复前加一个 task audit
做一次收尾审计
还有什么隐患？
你最没把握的地方是什么？
我有哪些没意识到的问题？
```

显式写 `$task-audit` 最可靠。

## 安装

把本仓库放到 Codex skills 目录下：

```bash
git clone https://github.com/diyewu/task-audit-skill.git ~/.codex/skills/task-audit
```

然后开启新的 Codex 会话，或刷新技能发现。

## 文件结构

```text
task-audit/
├── SKILL.md
└── agents/
    └── openai.yaml
```

当前版本没有 `scripts/`、`references/`、`assets/`。这个技能只需要一份流程协议。
