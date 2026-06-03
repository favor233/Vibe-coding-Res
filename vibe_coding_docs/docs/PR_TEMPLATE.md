# PR_TEMPLATE

## 1. Summary

简要说明这个 PR 做了什么。

```md
This PR adds the personalized learning plan creation API and related tests.
```

## 2. Related Task

链接或填写任务编号：

```text
Task:
Issue:
ADR:
```

## 3. Change Type

选择：

```md
- [ ] Feature
- [ ] Bug fix
- [ ] Refactor
- [ ] Documentation
- [ ] Test
- [ ] Security
- [ ] Infrastructure
```

## 4. Risk Level

```md
- [ ] Low
- [ ] Medium
- [ ] High
```

High risk 必须说明是否已人工确认。

## 5. What Changed

```md
- Added ...
- Updated ...
- Removed ...
```

## 6. What Did Not Change

说明刻意没有做的内容，防止误解。

```md
- Did not modify auth behavior.
- Did not change database schema.
- Did not add billing logic.
```

## 7. Screenshots / Recording

如果涉及 UI，请添加截图或录屏。

```md
Before:
After:
```

## 8. API Changes

```md
Endpoint:
Method:
Request:
Response:
Errors:
Auth:
```

## 9. Database Changes

```md
- [ ] No database changes
- [ ] Non-breaking migration
- [ ] Breaking migration
```

如果有 migration：

```md
Migration file:
Rollback plan:
Data risk:
```

## 10. Security Review

```md
- [ ] No security impact
- [ ] Auth checked
- [ ] Authorization checked
- [ ] Input validation added
- [ ] Output validation added
- [ ] Secrets not exposed
- [ ] Logs reviewed
- [ ] AI prompt injection considered
```

## 11. Tests

填写实际运行过的命令，不允许伪造。

```md
Commands run:
- npm run typecheck
- npm run lint
- npm run test
- npm run test:integration
- npm run build

Results:
- Typecheck:
- Lint:
- Unit:
- Integration:
- Build:
```

## 12. Manual Test Cases

```md
- [ ] Case 1:
- [ ] Case 2:
- [ ] Case 3:
```

## 13. Reviewer Checklist

```md
- [ ] Code is readable
- [ ] Change is minimal
- [ ] No unrelated refactor
- [ ] Module boundaries respected
- [ ] Tests are meaningful
- [ ] Error handling is safe
- [ ] Documentation updated
- [ ] Rollback plan is clear
```

## 14. AI Agent Disclosure

```md
Was AI used?
- [ ] Yes
- [ ] No

If yes:
- Agent/tool:
- Scope:
- Human review completed:
```

## 15. Known Limitations

```md
- 
- 
```

## 16. Follow-up Tasks

```md
- 
- 
```

## 17. Rollback Plan

```md
To rollback:
1. Revert this PR.
2. Run migration rollback if applicable.
3. Disable feature flag if applicable.
4. Monitor logs.
```

## 18. Final Confirmation

```md
- [ ] I have read the diff.
- [ ] I have run or reviewed the tests.
- [ ] I have checked security impact.
- [ ] I understand the rollback plan.
```
