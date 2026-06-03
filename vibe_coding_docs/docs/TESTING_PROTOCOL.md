# TESTING_PROTOCOL

## 1. 测试目标

测试不是为了证明代码“看起来能跑”，而是为了确保：

- 新功能符合需求
- 旧功能没有被破坏
- 边界情况被覆盖
- 错误处理可预期
- AI Agent 后续修改时有保护网

## 2. 测试分层

推荐测试金字塔：

```text
E2E Tests
Integration Tests
Unit Tests
Static Checks
```

### 2.1 Static Checks

包括：

- Type check
- Lint
- Format check
- Dependency audit
- Dead code check

每次 PR 必须运行。

### 2.2 Unit Tests

覆盖：

- 纯函数
- 领域逻辑
- 输入校验
- 状态转换
- 权限判断
- 错误分支

### 2.3 Integration Tests

覆盖：

- API + Database
- Auth + API
- AI Provider Adapter
- Payment Provider Adapter
- File upload
- Email / Notification

### 2.4 E2E Tests

覆盖用户关键路径：

- 注册 / 登录
- 创建学习目标
- 生成学习计划
- 完成课程
- 提交测验
- 查看反馈
- 查看进度

## 3. 测试命名规则

推荐：

```text
should_create_learning_plan_when_input_is_valid
should_reject_guest_when_endpoint_requires_auth
should_return_rate_limit_error_after_too_many_requests
```

禁止：

```text
test1
works
fix
caseA
```

## 4. 每个任务必须提供的测试

每个功能任务至少包含：

- 正常路径测试
- 一个边界测试
- 一个错误路径测试

示例：

```md
Feature: Create learning plan

Tests:
- Valid user can create learning plan.
- Empty goal returns validation error.
- Unauthenticated user receives 401.
```

## 5. Bug 修复测试规则

所有 bug 修复必须先添加一个会失败的测试，用来证明 bug 存在。

流程：

1. 写失败测试
2. 运行并确认失败
3. 修复代码
4. 运行并确认通过
5. 运行相关回归测试

## 6. 回归测试协议

每次合并前必须运行：

```bash
npm run typecheck
npm run lint
npm run test
npm run test:integration
```

涉及 UI 时额外运行：

```bash
npm run test:e2e
```

涉及安全时额外运行：

```bash
npm audit
```

## 7. AI 功能测试

AI 功能必须测试：

- Prompt 输入为空
- Prompt 输入超长
- Prompt injection
- Model timeout
- Provider 502 / 503
- 返回格式不合法
- 返回内容缺字段
- 用户无权限调用
- 调用频率过高

## 8. Mock 使用规则

允许 mock：

- 第三方 API
- AI Provider
- 邮件服务
- 支付沙盒
- 文件存储

禁止：

- 用 mock 掩盖核心业务错误
- 所有测试都 mock 数据库
- 集成测试中 mock 掉被测系统核心路径

## 9. 测试数据规则

测试数据必须：

- 可重复
- 可清理
- 不依赖真实用户
- 不包含真实隐私数据
- 不依赖执行顺序

## 10. 覆盖率要求

Prototype 阶段：

- 核心业务逻辑有测试
- 不强制整体覆盖率

MVP 阶段：

- Domain Layer 覆盖率建议 80%+
- Application Layer 覆盖主要流程
- API 关键路径必须有集成测试

Production 阶段：

- 核心模块覆盖率 85%+
- 所有高风险路径有 E2E 或集成测试
- 安全相关逻辑必须有负向测试

## 11. 手动测试清单

每次 UI 改动必须检查：

- 页面是否正常加载
- 表单校验是否正常
- Loading 状态是否正常
- Error 状态是否正常
- Empty 状态是否正常
- 移动端是否可用
- 刷新页面后状态是否合理
- 权限不足时是否被拦截

## 12. 测试报告格式

```md
## Test Summary

### Static Checks
- Typecheck: pass / fail
- Lint: pass / fail

### Unit Tests
- Command:
- Result:

### Integration Tests
- Command:
- Result:

### E2E Tests
- Command:
- Result:

### Manual Tests
- Case 1:
- Case 2:

### Known Gaps
- Gap 1:
- Gap 2:
```

## 13. 测试失败处理

测试失败时禁止：

- 删除测试
- 放宽断言
- 跳过测试
- 关闭 CI
- 伪造通过

必须：

- 说明失败原因
- 判断是代码问题还是测试问题
- 如果测试过时，解释为什么
- 修改后重新运行

## 14. CI 最低要求

CI 至少包含：

```yaml
- install dependencies
- typecheck
- lint
- unit tests
- integration tests
- build
```

Production 项目额外包含：

```yaml
- e2e tests
- dependency audit
- secret scan
- docker build
```
