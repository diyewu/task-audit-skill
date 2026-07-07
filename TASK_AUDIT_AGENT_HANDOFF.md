# task-audit Skill 交接文档

## 交接目标

这份文档给后续 agent 快速理解 `task-audit` skill：它解决什么问题、何时触发、如何使用、当前仓库里有哪些文件、下一步如果要扩展应该注意什么。

仓库地址：

```text
https://github.com/diyewu/task-audit-skill
```

本地路径：

```text
/Users/wudiye/develop/personal/code/skill-develop
```

## Skill 定位

`task-audit` 是一个 Codex 收尾审计 skill。

它不是普通总结，也不是测试工具。它的作用是在复杂任务完成或即将完成时，强制 agent 暴露两类信息：

1. Agent 当前最没有把握的事情。
2. 用户可能遗漏、误判或没有意识到的问题。

核心判断是：复杂任务最大的风险，往往不是没有完成，而是用户误以为已经闭环。

## 使用场景

适合使用：

- 复杂开发任务结束前。
- 多轮调研、排障、部署或文档沉淀收尾时。
- 跨仓库、跨工具、跨权限、跨系统的任务完成后。
- 有测试未跑、命令失败、验证跳过、事实依赖推断时。
- 用户问“还有什么隐患”“你最没把握什么”“我漏了什么”时。

不适合使用：

- 一次性简单问答。
- 小的格式调整。
- 单条命令结果说明。
- 没有明显风险面的轻量任务。

## 用户调用方式

最可靠的调用方式是显式写 `$task-audit`：

```text
用 $task-audit 收尾：你最没把握什么？我漏了什么？
```

也可以用自然语言触发：

```text
最终回复前加一个 task audit
做一次收尾审计
还有什么隐患？
你最没把握的地方是什么？
我有哪些没意识到的问题？
```

注意：自动触发只能尽力而为。跨会话或重要任务中，建议用户显式点名 `$task-audit`。

## 当前文件

```text
task-audit-skill/
├── SKILL.md
├── README.md
├── TASK_AUDIT_AGENT_HANDOFF.md
└── agents/
    └── openai.yaml
```

关键文件说明：

- `SKILL.md`：skill 主体，包含 frontmatter trigger 描述、审计流程、输出格式和质量标准。
- `agents/openai.yaml`：Codex UI 元数据，允许隐式触发。
- `README.md`：面向普通用户的简体中文说明。
- `TASK_AUDIT_AGENT_HANDOFF.md`：本文档，面向后续 agent。

当前没有 `scripts/`、`references/`、`assets/`。这是有意保留的最小实现，因为该 skill 的价值是流程协议，不是自动化脚本。

## 输出要求

默认输出结构：

```markdown
**Task Audit**

**最没把握**

1. 问题：
   根本原因：
   缺少证据：
   影响：
   建议：

**用户遗漏**

1. 问题：
   为什么是遗漏：
   影响：
   建议：

**优先级**

- 现在处理：
- 稍后处理：
- 可以忽略：
```

输出应短而具体。通常每个部分 1 条就够，最多 3 条。不要为了填格式编造风险。

## 质量标准

后续 agent 使用或修改这个 skill 时，必须保留这些原则：

- 每个风险必须绑定当前任务证据，不要泛泛提醒。
- 明确说“我没有验证 X”，不要用“建议关注”这种软话。
- 区分“没做”和“做了但仍有风险”。
- 不重复最终总结，只审计总结背后的弱点。
- 优先暴露会改变用户下一步行动的风险。
- 下一步建议要具体到命令、文件、责任人、检查项或验收标准。

## 已验证事项

当前版本已做过基础验证：

```text
quick_validate.py /Users/wudiye/develop/personal/code/skill-develop
```

结果：

```text
Skill is valid!
```

GitHub 远端：

```text
origin git@github.com:diyewu/task-audit-skill.git
branch main
```

## 后续扩展建议

暂时不要急着加脚本或复杂资源。

如果真实使用后发现输出泛泛，可以优先改 `SKILL.md`：

- 增加 2-3 个真实任务审计样例。
- 明确“泛泛风险清单”的反例。
- 强化“只抓会改变下一步行动的风险”。

只有当用户反复要求生成固定格式报告、批量审计日志或自动提取命令记录时，再考虑增加 `scripts/`。

## 给后续 agent 的一句话

维护这个 skill 时，别把它做成大而全的复盘系统。它最重要的能力只有一个：在任务看起来完成的时候，逼 agent 把最不确定的地方说出来。
