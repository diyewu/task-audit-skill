---
name: task-audit
description: Audit a completed or nearly completed complex task by exposing the agent's least-certain claims, missing evidence, user omissions, impact, and smallest next actions. Use when the user asks for task audit, 收尾审计, 风险审计, 隐患, 你最没把握什么, 我漏了什么, or before final response for substantial multi-step work involving code changes, debugging, research, deployment, documentation, cross-repo/tool/system facts, failed or skipped verification, assumptions, permissions, data/security, or handoff risk. Do not use for trivial one-shot answers, formatting edits, or a single command result.
---

# Task Audit

Task Audit is a closing audit, not a progress summary. Use it to make uncertainty visible before the user treats a complex task as done.

## Core Rule

Name the 1-3 material risks that could change the user's decision or next action.

If there is no material audit finding, say that directly and name the evidence that makes the task feel closed. Do not invent risks to fill the template.

## Audit Workflow

1. Reconstruct what is actually proven.
   - List the files, commands, tests, browser checks, logs, docs, or user-provided facts that support the result.
   - Mark any failed, skipped, partial, or impossible verification.
   - Treat memory, old docs, and inference as weaker evidence unless refreshed in the current task.

2. Find the agent's least-certain point.
   - Prefer the uncertainty with the highest impact, not the longest list.
   - Look for untested code paths, skipped commands, stale docs, ambiguous requirements, cross-repo facts, deployment state, permissions, data shape, security, compatibility, or maintenance cost.

3. Find the user's likely omission.
   - Focus on what the user may falsely assume is already settled.
   - Common omissions: acceptance criteria, rollout, rollback, monitoring, ownership, handoff, docs, data migration, credentials, privacy, compliance, cost, or future maintenance.

4. Recommend the smallest useful next action.
   - Prefer one concrete check, command, doc update, owner decision, or acceptance test.
   - Do not start a new large workstream inside the audit unless the user already asked you to continue.

## Output Format

Use this structure by default:

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

Keep each section short. Usually one item per section is enough; use up to three only when the risks are genuinely separate.

## Quality Bar

- Tie every finding to this task's evidence, not a generic checklist.
- Say "I did not verify X" instead of soft language like "may need attention."
- Separate "not done" from "done but risky."
- Do not repeat the final summary. Audit the weak points behind the summary.
- Do not bury the top risk under low-impact reminders.
- If the next action is obvious and cheap, name the exact command, file, owner, or decision.

## User Invocation

Users can trigger this skill explicitly with prompts like:

- `用 $task-audit 收尾`
- `做一次任务审计：你最没把握什么，我漏了什么？`
- `最终回复前加一个 task audit`
- `还有什么隐患？`

Explicit `$task-audit` invocation is the most reliable way to force the audit. Conditional automatic use is best-effort and depends on the task looking substantial from the current context.
