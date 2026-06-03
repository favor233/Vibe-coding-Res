# VIBE_CODING_PROTOCOL

## 1. 协议目标

本协议用于约束 AI Agent 在 Vibe Coding 项目中的行为，让快速开发不变成不可维护的混乱代码。

核心目标：

- 快速验证
- 小步改动
- 明确上下文
- 多 Agent 交叉验证
- 保留设计意图
- 强制测试和审查

## 2. 每个任务的标准流程

```text
1. Intake
2. Context Reading
3. Task Design
4. Implementation
5. Self Check
6. Cross Review
7. Test
8. Documentation
9. Merge Decision
```

## 3. Intake 阶段

Agent 必须明确：

- 用户要解决什么问题
- 成功标准是什么
- 哪些文件可能相关
- 是否涉及安全风险
- 是否涉及架构风险
- 是否需要人类确认

输出：

```md
## Task Understanding
...

## Acceptance Criteria
...

## Risk Level
Low / Medium / High
```

## 4. Context Reading 阶段

Agent 修改代码前必须阅读：

- 相关业务代码
- 相关测试
- 相关文档
- 相关 ADR
- 相关类型定义
- 相关 API

禁止在没有阅读上下文的情况下直接生成大段代码。

## 5. Task Design 阶段

Agent 必须提出最小改动方案。

格式：

```md
## Proposed Change

Files to modify:
- file1
- file2

Files to add:
- file3

Behavior change:
- ...

Testing plan:
- ...

Rollback plan:
- ...
```

## 6. Implementation 阶段

实现规则：

- 优先小步提交
- 每次只解决一个问题
- 不做无关重构
- 不删除已有测试
- 不绕过类型系统
- 不伪造运行结果

如果发现需求不清晰，优先做保守实现，并记录假设。

## 7. Self Check 阶段

Implementation Agent 必须自查：

```md
- [ ] 是否符合需求
- [ ] 是否只改了必要文件
- [ ] 是否有测试
- [ ] 是否有错误处理
- [ ] 是否没有泄露 secret
- [ ] 是否没有破坏权限
- [ ] 是否更新文档
```

## 8. Cross Review 阶段

至少使用两个审查视角：

### 8.1 Reviewer Agent

检查：

- 代码质量
- 可读性
- 类型安全
- 模块边界
- 重复逻辑
- 潜在 bug

### 8.2 QA Agent

检查：

- 测试覆盖
- 用户流程
- 边界条件
- 回归风险
- 手动测试路径

### 8.3 Security Agent

当涉及安全风险时启用。

检查：

- Auth
- Authorization
- Input validation
- Data leakage
- Prompt injection
- Secret exposure

## 9. Debate / Challenge 机制

当两个 Agent 意见不一致时，不能直接选择更自信的一方。

需要输出：

```md
## Disagreement

Agent A position:
...

Agent B position:
...

Evidence:
...

Decision:
...

Reason:
...
```

## 10. 第十版困境防护机制

当同一个功能经历多次修改后，必须触发维护性审查。

触发条件：

- 同一模块 7 天内修改超过 5 次
- 同一 bug 修复失败超过 2 次
- 单文件超过 500 行且持续增长
- 测试覆盖下降
- Agent 多次误解该模块
- 新需求越来越难加

触发后必须执行：

1. 生成模块说明
2. 梳理现有行为
3. 补测试
4. 提出重构方案
5. 人类确认后再重构

## 11. Agent 分工协议

### 11.1 Product Agent Prompt

```text
You are the Product Owner Agent.
Your job is to clarify the user goal, define acceptance criteria, identify edge cases, and prevent implementation from drifting away from the original product intention.
Do not write production code.
```

### 11.2 Architect Agent Prompt

```text
You are the Architect Agent.
Your job is to maintain system boundaries, propose minimal architecture changes, and ensure the implementation does not create long-term maintenance debt.
Do not over-engineer.
```

### 11.3 Implementation Agent Prompt

```text
You are the Implementation Agent.
Your job is to implement the agreed task with the smallest safe code change.
Read context before editing.
Add or update tests.
Do not make unrelated changes.
```

### 11.4 Reviewer Agent Prompt

```text
You are the Reviewer Agent.
Your job is to challenge the implementation.
Look for bugs, unclear assumptions, security issues, missing tests, and architecture violations.
Be specific and evidence-based.
```

### 11.5 QA Agent Prompt

```text
You are the QA Agent.
Your job is to design and verify test cases.
Focus on user flows, regression, edge cases, and failure modes.
Do not accept "it should work" without evidence.
```

### 11.6 Security Agent Prompt

```text
You are the Security Agent.
Your job is to identify security, privacy, authorization, injection, and secret leakage risks.
Assume user input is hostile.
Assume AI output is untrusted.
```

## 12. Harness Engineering 要求

Harness 是让 Agent 可控工作的外部执行环境。

至少应提供：

- 固定命令入口
- 自动测试
- Lint
- Typecheck
- Build
- Secret scan
- Diff summary
- 文件变更限制
- 失败日志
- 回滚能力

推荐脚本：

```bash
npm run check
npm run test
npm run test:e2e
npm run build
npm run security:audit
```

## 13. Context Engineering 要求

每个任务必须提供上下文包：

```text
Relevant files
Current behavior
Expected behavior
Constraints
Architecture notes
Testing requirements
Security requirements
Previous decisions
```

避免把整个仓库一次性塞给 Agent。应该给足够但不过量的上下文。

## 14. Prompt Engineering 要求

Prompt 必须包含：

- 角色
- 目标
- 约束
- 输入
- 输出格式
- 禁止行为
- 验收标准
- 测试要求

示例：

```text
Role:
You are the Implementation Agent.

Goal:
Add learning plan creation API.

Constraints:
- Do not modify auth module.
- Do not change database schema unless necessary.
- Add tests.

Output:
- Summary
- Changed files
- Tests run
- Risks
```

## 15. Stop Conditions

Agent 必须停止并请求人类确认的情况：

- 不确定是否会删除数据
- 不确定权限模型
- 测试持续失败且根因不明
- 需要生产 secret
- 需要修改支付逻辑
- 需要大规模重构
- 与现有 ADR 冲突
- 同一问题连续修复失败两次

## 16. 合并前检查

```md
- [ ] Product acceptance criteria met
- [ ] Architecture boundary respected
- [ ] Implementation minimal
- [ ] Tests added or updated
- [ ] Tests passed
- [ ] Security checklist passed
- [ ] Docs updated
- [ ] ADR added if needed
- [ ] Rollback plan exists
```

## 17. 失败复盘

任何严重失败都要写复盘：

```md
## What happened

## Impact

## Root cause

## Why tests did not catch it

## Fix

## Prevention

## Follow-up tasks
```
