# SECURITY_CHECKLIST

## 1. 安全目标

本清单用于降低 Vibe Coding 中常见的安全风险：

- Agent 为了快速实现功能绕过权限
- 输入未经校验直接进入数据库或 AI Prompt
- Secret 泄露到前端或日志
- 第三方依赖带来漏洞
- AI 功能被 prompt injection 攻击
- 用户数据被越权访问

## 2. 认证 Authentication

检查项：

- 是否所有私有 API 都要求登录
- Session / Token 是否有过期时间
- Refresh Token 是否安全存储
- 登录失败是否有限流
- 密码是否使用安全 hash
- 是否禁止明文密码存储
- 是否支持登出后 token 失效

禁止：

- 在 localStorage 存放高权限长期 token
- 在 URL 中传递 token
- 将密码写入日志

## 3. 授权 Authorization

检查项：

- 用户是否只能访问自己的数据
- 管理员接口是否有角色检查
- 后端是否重复验证权限
- 前端隐藏按钮不能作为唯一权限控制
- 是否存在 IDOR 风险

典型风险：

```text
GET /api/users/123/progress
```

如果用户可以把 123 改成 124 看到别人数据，就是 IDOR。

## 4. 输入校验

所有外部输入必须校验：

- Body
- Query
- Params
- Headers
- Cookies
- File upload
- Webhook payload
- AI Prompt input

必须校验：

- 类型
- 长度
- 格式
- 枚举值
- 数字范围
- 是否允许为空

## 5. 输出编码

检查项：

- 是否防止 XSS
- 用户输入是否被安全渲染
- Markdown 是否经过 sanitizer
- HTML 是否禁止直接注入
- 文件名是否安全处理

禁止：

- 直接使用 `dangerouslySetInnerHTML` 渲染用户输入
- 直接把 AI 输出当 HTML 渲染
- 直接拼接脚本内容

## 6. 数据库安全

检查项：

- 是否使用参数化查询
- 是否避免 SQL 拼接
- Migration 是否可审计
- 是否限制生产数据库权限
- 是否有备份
- 是否有数据删除策略

禁止：

```ts
db.query("SELECT * FROM users WHERE id = " + userInput)
```

推荐：

```ts
db.query("SELECT * FROM users WHERE id = ?", [userInput])
```

## 7. Secret 管理

Secret 包括：

- API Key
- Database URL
- JWT Secret
- OAuth Secret
- Payment Secret
- Webhook Secret
- AI Provider Key

检查项：

- 是否只存在环境变量或 secret manager
- 是否没有提交到 Git
- 是否没有进入前端 bundle
- 是否没有出现在日志
- 是否有轮换机制

## 8. 日志安全

日志不得包含：

- 密码
- Token
- API Key
- 完整身份证
- 完整银行卡
- 私密聊天内容
- 未脱敏 Prompt
- 用户上传的敏感文件内容

日志可以包含：

- requestId
- userId
- action
- status
- duration
- errorCode

## 9. 文件上传安全

检查项：

- 文件大小限制
- 文件类型白名单
- 文件名清理
- 病毒扫描，如果进入生产
- 私有文件访问鉴权
- 禁止直接执行上传文件
- 图片处理防止 metadata 泄露

## 10. AI 安全

AI 功能必须检查：

- Prompt injection
- 数据越权注入
- 用户诱导模型泄露系统提示
- AI 输出是否需要校验
- AI 是否可能生成危险操作
- 工具调用是否有权限边界

### 10.1 Prompt Injection 防护

不得把用户内容和系统指令混在一个无边界字符串中。

推荐结构：

```text
System Instruction:
You are a learning assistant.

Developer Instruction:
Follow the learning plan format.

User Content:
<user_input>
```

### 10.2 AI 输出校验

AI 返回 JSON 时必须 parse + schema validate。

禁止：

- 直接信任 AI 输出
- AI 说成功就认为成功
- AI 生成 SQL 后直接执行
- AI 生成 shell 命令后直接运行

## 11. 第三方依赖

检查项：

- 是否使用维护中的包
- 是否有已知 CVE
- 是否锁定版本
- 是否来自可信来源
- 是否引入不必要的大依赖
- 是否有 postinstall 脚本风险

## 12. Webhook 安全

必须：

- 验证签名
- 验证时间戳
- 防止重放攻击
- 记录 eventId
- 保证幂等
- 不信任 payload 中的用户身份

## 13. Rate Limit

需要限流的接口：

- 登录
- 注册
- 发送验证码
- AI 生成
- 文件上传
- 支付创建
- 搜索
- Webhook

## 14. 错误信息安全

用户可见错误应该简洁。

禁止返回：

- Stack trace
- SQL 错误
- Secret
- 内部文件路径
- Provider 原始错误详情

## 15. 安全审查触发条件

以下改动必须进行 Security Agent 审查：

- Auth 相关改动
- 权限相关改动
- 数据库查询改动
- AI 工具调用改动
- 文件上传
- 支付
- Webhook
- 日志系统
- Secret 配置
- 新增第三方 SDK

## 16. 发布前安全清单

```md
- [ ] 所有私有 API 有认证
- [ ] 所有资源访问有授权
- [ ] 所有输入有 schema 校验
- [ ] 所有 AI 输出有结构校验
- [ ] 没有 secret 进入代码库
- [ ] 没有敏感信息进入日志
- [ ] 依赖安全扫描通过
- [ ] 文件上传有限制
- [ ] Webhook 有签名验证
- [ ] Rate limit 已配置
- [ ] 错误信息不泄露内部细节
```
