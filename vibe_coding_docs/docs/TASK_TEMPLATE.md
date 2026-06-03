# TASK_TEMPLATE

## 1. Task Title

简短描述任务。

示例：

```text
Add personalized learning plan generation API
```

## 2. Background

说明为什么要做这个任务。

```md
当前用户可以输入学习目标，但系统还不能自动生成个性化学习计划。
本任务要新增一个 API，根据用户目标、可用时间和当前水平生成学习计划。
```

## 3. User Story

```md
As a user,
I want to generate a personalized learning plan,
so that I can know what to learn next and track my progress.
```

## 4. Goal

本任务要达成的具体目标：

- 
- 
- 

## 5. Non-Goals

本任务不包含：

- 
- 
- 

## 6. Acceptance Criteria

```md
- [ ] 用户登录后可以提交学习目标
- [ ] 系统返回结构化学习计划
- [ ] 无效输入返回 400
- [ ] 未登录用户返回 401
- [ ] AI Provider 失败时返回可理解错误
- [ ] 相关测试通过
```

## 7. Relevant Files

```text
apps/api/src/modules/learning-plans/
apps/api/src/modules/ai-tutor/
apps/web/src/pages/learning-plan/
docs/ARCHITECTURE.md
```

## 8. Constraints

```md
- 不允许修改 auth 模块
- 不允许修改现有用户表结构
- 不允许在前端暴露 AI Provider Key
- 必须添加测试
```

## 9. Risk Level

选择一个：

```text
Low / Medium / High
```

判断标准：

- Low：UI、文案、小修复
- Medium：API、数据库查询、多模块调用
- High：权限、支付、数据删除、schema 破坏性修改

## 10. Implementation Plan

```md
1. Add request schema.
2. Add application service.
3. Add AI provider interface.
4. Add API route.
5. Add unit tests.
6. Add integration tests.
7. Update docs.
```

## 11. Test Plan

```md
Unit tests:
- valid input creates plan
- empty goal fails
- invalid available hours fails

Integration tests:
- logged-in user can create plan
- guest receives 401
- provider failure returns safe error

Manual tests:
- submit form from UI
- check loading state
- check error state
```

## 12. Security Checklist

```md
- [ ] Auth required
- [ ] User can only create plan for self
- [ ] Input schema validated
- [ ] AI output schema validated
- [ ] No secret exposed to client
- [ ] No sensitive data logged
```

## 13. Documentation Updates

```md
- [ ] README
- [ ] API docs
- [ ] ADR if architecture changed
- [ ] Module README
```

## 14. Rollback Plan

```md
If the feature causes issues:
1. Disable API route via feature flag.
2. Revert PR.
3. Keep database migration if non-breaking.
4. Notify affected users if needed.
```

## 15. Agent Assignment

```md
Product Agent:
Architect Agent:
Implementation Agent:
Reviewer Agent:
QA Agent:
Security Agent:
Documentation Agent:
```

## 16. Final Report

```md
## Summary

## Files Changed

## Tests Run

## Risks

## Follow-up Tasks
```
