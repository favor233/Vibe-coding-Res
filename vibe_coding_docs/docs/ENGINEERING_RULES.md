# ENGINEERING_RULES

## 1. 总原则

本项目的工程规则服务于一个目标：让 AI 生成的代码可以持续维护。

每一次代码修改都必须满足：

- 可读
- 可测
- 可追踪
- 可回滚
- 可解释
- 不破坏旧功能

## 2. 代码风格

### 2.1 命名

命名必须表达业务含义。

推荐：

```ts
calculateLearningProgress()
createAssessmentSession()
validateUserPermission()
```

禁止：

```ts
doThing()
handleData()
process()
fixBug()
newFunction()
```

### 2.2 函数长度

建议：

- 普通函数不超过 40 行
- 复杂函数必须拆分
- 单个函数只做一件事
- 超过 3 层嵌套需要重构

### 2.3 类型规则

必须：

- API 输入输出有明确类型
- 数据库实体有明确类型
- 领域对象有明确类型
- 外部服务响应必须经过 parse / validation

禁止：

- 随意使用 `any`
- 未校验外部 API 返回
- 将未知对象直接传入核心逻辑

## 3. 注释规则

注释应该解释“为什么”，不是重复“做了什么”。

推荐：

```ts
// We retry once because the AI provider sometimes returns transient 502 errors.
```

不推荐：

```ts
// Increment i by 1.
```

复杂业务规则必须写注释。

临时代码必须标记：

```ts
// TODO: Replace mock provider before production.
// TEMP: Used only for prototype validation.
// FIXME: This does not handle timezone correctly.
```

## 4. 文件组织

每个文件只承担一个清晰职责。

禁止：

- 一个文件同时包含 UI、数据库和业务逻辑
- 工具函数文件无限膨胀
- 把所有类型塞进 `types.ts`
- 把所有 API 都写在一个文件里

## 5. 提交规则

每次提交应该是一个完整、可理解的变更单元。

推荐提交格式：

```text
feat: add learning plan creation flow
fix: handle expired assessment session
test: add quiz scoring edge cases
docs: add ADR for vector database decision
refactor: split AI provider interface
```

禁止：

```text
update
fix
changes
wip
final
final2
really final
```

## 6. 分支规则

推荐分支命名：

```text
feature/learning-plan-generator
fix/assessment-timeout
refactor/ai-provider-interface
docs/vibe-coding-protocol
```

高风险改动必须单独分支，不得混入普通功能。

## 7. 代码生成规则

AI Agent 生成代码时必须遵守：

1. 先读相关文件
2. 再提出最小改动方案
3. 实现前说明会改哪些文件
4. 优先小步修改
5. 每次修改后运行测试
6. 如果测试失败，先分析再修
7. 不允许盲目大规模重写

## 8. 禁止行为

AI Agent 禁止：

- 删除大量代码但不说明原因
- 为了通过测试而删除测试
- 为了消除错误而关闭类型检查
- 为了省事而绕过权限
- 将 secret 写入代码
- 复制粘贴重复逻辑
- 伪造测试通过结果
- 声称运行了测试但没有实际运行
- 用 mock 掩盖真实集成问题
- 把所有 bug 都归因于环境问题

## 9. Bug 修复规则

修 bug 必须遵循：

1. 复现问题
2. 找到根因
3. 添加失败测试
4. 修复代码
5. 验证测试通过
6. 检查是否影响相关功能
7. 记录修复说明

Bug 修复报告格式：

```md
## Bug
描述问题。

## Root Cause
说明根因。

## Fix
说明改动。

## Tests
说明新增或运行的测试。

## Risk
说明潜在影响。
```

## 10. 重构规则

重构必须满足：

- 不改变外部行为
- 先有测试保护
- 一次只做一个方向的重构
- 不混入新功能
- 保留清晰迁移说明

禁止：

- “顺手”重写整个模块
- 重构同时改业务逻辑
- 没有测试就重构核心代码
- 只因为代码不好看就大改

## 11. 依赖管理

新增依赖前必须回答：

- 为什么需要这个依赖？
- 是否可以用现有依赖完成？
- 是否仍在维护？
- License 是否允许？
- Bundle size 是否可接受？
- 是否有安全漏洞？
- 是否会锁死未来架构？

依赖新增必须记录在 PR 描述中。

## 12. 配置管理

配置必须通过环境变量或配置文件管理。

禁止：

- 硬编码 API Key
- 硬编码生产 URL
- 在前端暴露后端 secret
- 将 `.env` 提交到仓库

推荐：

```text
.env.example
.env.local
.env.production
```

## 13. 数据迁移规则

数据库 migration 必须：

- 可重复执行或具备幂等保护
- 有回滚方案
- 在测试环境验证
- 说明是否破坏旧数据
- 对大表变更进行风险评估

## 14. Pull Request 最低标准

PR 必须包含：

- 改动目的
- 改动范围
- 测试结果
- 风险说明
- 回滚方案
- 截图或录屏，如果涉及 UI
- ADR，如果涉及架构决策

## 15. Definition of Done

一个任务完成必须满足：

- 功能实现
- 测试通过
- 文档更新
- 无明显安全问题
- 无无关改动
- Reviewer Agent 审查通过
- 人类确认高风险部分
