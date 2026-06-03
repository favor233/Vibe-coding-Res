# ARCHITECTURE

## 1. 架构目标

本项目架构的核心目标是：

- 支持快速迭代
- 保持模块边界清晰
- 允许 AI Agent 安全修改局部代码
- 降低后期维护成本
- 让功能、数据、权限和测试之间有明确关系
- 避免 Vibe Coding 后期出现“没人知道为什么这么写”的问题

## 2. 推荐架构风格

推荐采用模块化单体优先，而不是一开始就拆成微服务。

原因：

- MVP 阶段需求变化快
- 多服务会增加部署、鉴权、日志、测试复杂度
- Agent 更容易在单体项目中理解上下文
- 模块边界清晰后，未来仍可拆分服务

## 3. 分层结构

推荐使用以下分层：

```text
UI Layer
  ↓
Application Layer
  ↓
Domain Layer
  ↓
Infrastructure Layer
```

### 3.1 UI Layer

职责：

- 页面展示
- 用户输入
- 表单验证
- 交互状态
- 调用应用服务

禁止：

- 直接访问数据库
- 写复杂业务逻辑
- 处理权限核心逻辑
- 拼接底层 SQL

### 3.2 Application Layer

职责：

- 编排业务流程
- 调用领域服务
- 处理事务
- 处理权限检查
- 协调外部服务

示例：

```text
createLearningPlan(userId, skillGoal)
submitQuizAnswer(userId, questionId, answer)
generateLessonFeedback(userId, lessonId)
```

### 3.3 Domain Layer

职责：

- 核心业务规则
- 领域对象
- 状态转换
- 业务不变量

示例：

```text
User
LearningPlan
SkillAssessment
Lesson
Quiz
Feedback
ProgressRecord
```

禁止：

- 依赖 UI
- 依赖具体数据库实现
- 依赖 HTTP 框架
- 依赖第三方 SDK

### 3.4 Infrastructure Layer

职责：

- 数据库访问
- 文件存储
- 外部 API
- 消息队列
- 邮件服务
- AI Model Provider

禁止：

- 写核心业务决策
- 直接被 UI 调用
- 泄露底层错误给用户

## 4. 模块边界

每个模块必须有明确的输入、输出和所有权。

示例模块：

```text
modules/
├── auth/
├── users/
├── learning-plans/
├── assessments/
├── lessons/
├── ai-tutor/
├── progress/
├── billing/
└── notifications/
```

每个模块内部建议结构：

```text
module-name/
├── api/
├── application/
├── domain/
├── infrastructure/
├── tests/
└── README.md
```

## 5. 数据流

典型请求流程：

```text
User Action
  ↓
UI Component
  ↓
API Route / Controller
  ↓
Application Service
  ↓
Domain Logic
  ↓
Repository / External Provider
  ↓
Response DTO
  ↓
UI Render
```

## 6. AI Agent 修改代码的边界规则

AI Agent 修改代码前必须判断改动类型。

### 6.1 Safe Change

可以直接进行：

- UI 文案调整
- 小型组件修复
- 单个函数 bug 修复
- 添加测试
- 增加非破坏性日志
- 更新文档

### 6.2 Medium Risk Change

需要 Reviewer Agent 审查：

- 修改 API 入参出参
- 修改数据库查询
- 修改状态管理
- 修改多个模块之间的调用
- 改动认证流程的一部分

### 6.3 High Risk Change

必须人类确认：

- 数据库 schema 破坏性修改
- 权限系统重写
- 支付逻辑修改
- 删除用户数据
- 大规模重构
- 替换核心依赖
- 修改生产配置

## 7. 依赖规则

允许方向：

```text
UI → Application → Domain
Application → Infrastructure
Infrastructure → Domain Interface
```

禁止方向：

```text
Domain → UI
Domain → Infrastructure Implementation
Infrastructure → UI
UI → Database
```

## 8. API 设计规则

所有 API 必须定义：

- 请求参数
- 响应结构
- 错误码
- 权限要求
- 幂等性要求
- 速率限制
- 日志策略

示例：

```md
POST /api/learning-plans

Purpose:
Create a personalized learning plan.

Auth:
Required.

Request:
{
  "goal": "learn Python basics",
  "availableHoursPerWeek": 5,
  "targetDate": "2026-08-01"
}

Response:
{
  "planId": "lp_123",
  "status": "created"
}

Errors:
- 400 INVALID_GOAL
- 401 UNAUTHORIZED
- 429 RATE_LIMITED
- 500 INTERNAL_ERROR
```

## 9. 数据库设计规则

每个表必须说明：

- 表用途
- 主键
- 外键
- 索引
- 是否包含敏感数据
- 删除策略
- 审计字段

推荐基础字段：

```text
id
created_at
updated_at
deleted_at
created_by
updated_by
```

## 10. 错误处理规则

错误分为：

- User Error：用户输入错误
- Auth Error：认证授权错误
- Business Error：业务规则不满足
- Dependency Error：第三方服务失败
- System Error：系统未知异常

禁止：

- 把原始数据库错误直接返回给用户
- 在日志中输出 token、密码、密钥
- 静默吞掉异常
- 用 `any` 或空 catch 掩盖问题

## 11. 日志和可观测性

关键操作必须记录：

- requestId
- userId
- action
- result
- duration
- errorCode
- dependencyName

不记录：

- 密码
- Token
- API Key
- 完整身份证件
- 完整银行卡
- 敏感 prompt 原文，除非脱敏

## 12. 架构变更流程

以下改动必须新增 ADR：

- 新增核心模块
- 替换数据库
- 替换 AI Provider
- 引入队列
- 引入缓存
- 权限模型变化
- 数据模型重大变化
- 从单体拆服务

ADR 文件命名：

```text
docs/ADR/0001-use-modular-monolith.md
docs/ADR/0002-add-vector-database.md
```
