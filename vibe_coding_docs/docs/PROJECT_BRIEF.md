# PROJECT_BRIEF

## 1. 项目目标

本项目的目标是建立一套适合 Vibe Coding 的工程化协作框架，让 AI Agent 可以快速完成产品原型、功能迭代和代码实现，同时尽量避免常见问题：

- 前 1-3 版开发速度很快，但后续第 10 版开始失控。
- 代码能跑，但没人知道为什么这么写。
- Bug 出现后只能继续让 AI 硬改，越改越乱。
- 缺少架构边界、测试协议、回归检查、安全审查和变更记录。
- 多个 Agent 之间只是在“并行写代码”，没有形成真正的交叉验证。

本项目不是要否定 Vibe Coding，而是把 Vibe Coding 从“随意生成代码”升级为“有约束、有审查、有回归、有决策记录的 AI 工程工作流”。

## 2. 项目原则

### 2.1 快速验证优先

在前期探索阶段，允许快速生成 Demo、MVP、实验性功能。重点是验证需求、交互、业务闭环和技术可行性。

### 2.2 可维护性必须从第一版开始埋点

即使是 Vibe Coding，也不能等到第十版才开始重构。每一次功能生成都必须留下：

- 设计意图
- 输入输出边界
- 测试用例
- 失败场景
- 依赖说明
- 后续风险

### 2.3 Agent 可以替代执行，但不能替代工程纪律

Agent 可以承担开发、测试、审查、文档、安全检查等角色，但必须在明确协议下工作。不能让所有 Agent 都自由发挥。

### 2.4 人类仍然是产品方向和最终风险的负责人

Multi-agent 可以提高覆盖率，但不能完全消除错误。人类至少需要监督：

- 产品目标是否正确
- 架构方向是否合理
- 高风险操作是否可接受
- 安全、隐私、支付、用户数据相关改动
- 是否进入“越修越乱”的状态

## 3. 适用范围

本套文档适用于以下类型项目：

- AI 学习系统
- SaaS MVP
- Web App
- 数据驱动产品
- 多 Agent 自动化开发项目
- Codex / Cursor / Claude Code / Devin 类 AI 编程工作流
- 需要快速验证但未来希望可维护的产品

## 4. 不适用范围

本套协议不适合直接用于以下场景，除非额外强化审查：

- 医疗诊断系统
- 金融交易系统
- 高并发核心交易系统
- 涉及大规模隐私数据处理的系统
- 工业控制、自动驾驶、生命安全系统

这些项目需要更严格的形式化验证、安全审计、合规审查和人工工程团队介入。

## 5. 推荐 Agent 角色

### 5.1 Product Owner Agent

职责：

- 拆解用户需求
- 明确用户故事
- 定义验收标准
- 判断是否偏离原始目标
- 防止 AI 只顾实现功能而忽略产品价值

输出：

- PRD
- User Story
- Acceptance Criteria
- Edge Cases

### 5.2 Architect Agent

职责：

- 设计系统结构
- 划分模块边界
- 评估技术选型
- 维护架构一致性
- 发现过度耦合和不可维护风险

输出：

- Architecture Decision Record
- Module Boundary
- Dependency Map
- Data Flow

### 5.3 Implementation Agent

职责：

- 编写代码
- 修改功能
- 修复 bug
- 实现测试
- 遵守工程规范

输出：

- Code Changes
- Unit Tests
- Migration Scripts
- Implementation Notes

### 5.4 Reviewer Agent

职责：

- 审查代码质量
- 查找隐藏 bug
- 检查需求是否被正确实现
- 检查是否违反架构规则

输出：

- Code Review Report
- Risk List
- Refactor Suggestions

### 5.5 QA Agent

职责：

- 设计测试用例
- 执行回归测试
- 验证边界场景
- 检查用户流程是否完整

输出：

- Test Plan
- Manual Test Checklist
- Regression Report

### 5.6 Security Agent

职责：

- 检查认证、授权、输入校验、数据泄露风险
- 检查依赖漏洞
- 检查日志是否泄露敏感信息
- 检查 AI 功能是否可能被 prompt injection 攻击

输出：

- Security Checklist
- Vulnerability Report
- Mitigation Plan

### 5.7 Documentation Agent

职责：

- 维护 README
- 更新 API 文档
- 更新变更说明
- 更新任务上下文
- 记录设计意图

输出：

- README Update
- API Docs
- Changelog
- ADR

## 6. 推荐开发流程

每一个任务必须经过以下阶段：

1. Clarify：明确需求与边界
2. Design：提出实现方案
3. Implement：实现最小可行改动
4. Test：运行自动化和手动测试
5. Review：交叉审查代码和架构
6. Document：补充文档和变更记录
7. Merge：合并到主分支
8. Observe：观察运行结果和用户反馈

## 7. 最小项目成功标准

一个 Vibe Coding 项目不能只以“功能能跑”为成功标准。最低成功标准是：

- 功能符合用户目标
- 代码可以被其他 Agent 或人类读懂
- 关键路径有测试
- 有错误处理
- 有安全边界
- 有架构记录
- 有回归验证
- 新功能不会破坏旧功能

## 8. 项目目录建议

```text
/
├── apps/
│   ├── web/
│   └── api/
├── packages/
│   ├── shared/
│   ├── ui/
│   └── config/
├── docs/
│   ├── PROJECT_BRIEF.md
│   ├── ARCHITECTURE.md
│   ├── ENGINEERING_RULES.md
│   ├── TESTING_PROTOCOL.md
│   ├── SECURITY_CHECKLIST.md
│   ├── VIBE_CODING_PROTOCOL.md
│   ├── TASK_TEMPLATE.md
│   ├── PR_TEMPLATE.md
│   └── ADR/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── scripts/
├── .github/
│   └── workflows/
├── README.md
└── package.json
```

## 9. 人类监督点

以下情况必须由人类确认：

- 数据库 schema 大改
- 权限系统变更
- 支付、订单、钱包、积分逻辑
- 删除数据
- 用户隐私数据处理
- 依赖大规模替换
- 架构重构
- 引入外部 API 或第三方服务
- 修改安全策略
- 连续两次修复同一 bug 失败

## 10. 版本阶段划分

### Prototype 阶段

目标：快速验证想法。

允许：

- 简单架构
- Mock 数据
- 快速 UI
- 低覆盖率测试

不允许：

- 无文档
- 无边界
- 无法复现运行
- 把临时代码伪装成正式架构

### MVP 阶段

目标：形成可测试、可迭代的产品。

必须：

- 有基础测试
- 有错误处理
- 有 API 文档
- 有数据模型说明
- 有基本安全检查

### Production 阶段

目标：可维护、可监控、可扩展。

必须：

- CI/CD
- E2E 测试
- 监控告警
- 权限审计
- 日志策略
- 回滚方案
- 安全审计
