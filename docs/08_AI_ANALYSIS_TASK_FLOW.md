# 08_AI_ANALYSIS_TASK_FLOW

## 文档信息

| 项目 | 内容 |
|---|---|
| 文档名称 | AI 分析任务流程设计 |
| 项目 | AI Violin Platform |
| 版本 | v0.1 |
| 状态 | FREEZE |
| 负责人 | ChatGPT |
| 审批人 | 项目经理 |
| 确认日期 | 2026-07-11 |

---

## 1. 文档目的

本文档定义 AI Violin Platform MVP 中，从用户提交练习到 AI 结果展示的完整异步任务流程。

---

## 2. 设计原则

- 异步分析。
- 主任务与子任务分离。
- 音准、节奏、姿势、运弓并行执行。
- 允许部分成功。
- 单模块可重试。
- 结构化结果优先保存。
- 自然语言反馈独立生成。
- 系统可扩展但第一版不过度设计。

---

## 3. 总体流程

```text
用户结束练习
↓
APP 生成音频和视频文件
↓
创建 practice_session
↓
上传音频和视频
↓
校验媒体文件
↓
创建 AI 主任务
↓
媒体预处理
↓
并行执行：
- pitch_analysis
- rhythm_analysis
- posture_analysis
- bowing_analysis
↓
汇总结构化结果
↓
计算综合结果
↓
生成自然语言反馈
↓
保存结果和反馈
↓
更新 practice_session 状态
↓
APP 展示分析结果
```

---

## 4. 主任务结构

主任务类型：

`full_practice_analysis`

主任务负责：

- 检查媒体文件
- 调度媒体预处理
- 创建和调度子任务
- 监控任务状态
- 汇总结果
- 触发反馈生成
- 更新练习状态
- 处理失败和部分成功

---

## 5. 子任务结构

子任务包括：

| 子任务 | 职责 |
|---|---|
| media_preprocessing | 校验和预处理音频、视频，为后续分析提供统一输入。 |
| pitch_analysis | 基于预处理音频完成音高与音准分析。 |
| rhythm_analysis | 基于预处理音频完成节拍、节奏稳定性和偏差分析。 |
| posture_analysis | 基于预处理视频完成基础演奏姿势识别。 |
| bowing_analysis | 基于预处理视频完成基础运弓动作分析。 |
| feedback_generation | 基于已保存的结构化结果生成自然语言练习反馈。 |

---

## 6. 媒体预处理

音频预处理包括：

- 文件格式校验
- 音频提取
- 采样率统一
- 声道统一
- 基础降噪
- 音量检查
- 时长检查

视频预处理包括：

- 视频方向纠正
- 分辨率检查
- 帧率检查
- 抽帧
- 音视频时间轴同步
- 画面完整性基础检查

第一版不追求复杂媒体增强。

---

## 7. 并行执行

媒体预处理完成后，并行执行：

- pitch_analysis
- rhythm_analysis
- posture_analysis
- bowing_analysis

其中：

- 音准和节奏共享预处理音频。
- 姿势和运弓共享预处理视频。
- 单个模块失败不应阻塞其他模块。
- 所有模块进入终态后才开始结果汇总。

---

## 8. AI 任务状态机

ai_analysis_tasks.task_status 使用：

| 状态 | 含义 |
|---|---|
| created | 已创建，尚未进入上传或调度流程。 |
| waiting_for_upload | 等待练习媒体上传。 |
| uploading | 媒体文件正在上传。 |
| queued | 已进入任务队列，等待执行。 |
| preprocessing | 正在进行媒体预处理。 |
| processing | 正在执行分析子任务。 |
| aggregating | 正在汇总结构化分析结果。 |
| generating_feedback | 正在生成自然语言反馈。 |
| completed | 所有必要模块完成并已生成反馈。 |
| partially_completed | 至少一个分析模块成功，且存在模块未完成或失败。 |
| failed | 分析任务未产生可展示结果。 |
| cancelled | 任务已被取消。 |

---

## 9. 练习记录状态

practice_sessions.status 使用：

| 状态 | 含义 |
|---|---|
| recording | 用户正在录制练习。 |
| uploading | 练习媒体正在上传。 |
| submitted | 媒体已提交，等待创建或调度分析任务。 |
| analyzing | AI 分析正在进行。 |
| analyzed | AI 分析和反馈已完成。 |
| partially_analyzed | AI 部分模块分析成功，仍可展示有效结果。 |
| failed | AI 分析失败，未产生可展示结果。 |
| deleted | 练习记录已删除。 |

任务细节状态由 ai_analysis_tasks 管理，业务展示状态由 practice_sessions 管理。

---

## 10. 部分成功策略

例如：

- 音准：成功
- 节奏：成功
- 姿势：成功
- 运弓：失败

主任务应进入：

`partially_completed`

练习记录应进入：

`partially_analyzed`

前端展示成功结果，并明确提示未生成的模块。

---

## 11. 失败处理

| 情况 | 处理方式 |
|---|---|
| 上传失败 | 标记上传失败，并允许用户重新上传。 |
| 音频不可用 | 标记依赖音频的模块失败，其他可执行模块继续。 |
| 视频不可用 | 标记依赖视频的模块失败，其他可执行模块继续。 |
| 单个分析模块失败 | 保留其他成功结果，主任务按部分成功处理。 |
| 所有分析模块失败 | 主任务和练习记录进入 failed。 |
| 反馈生成失败 | 保留结构化结果，允许单独重试 feedback_generation。 |
| 数据库保存失败 | 标记任务失败并记录失败原因，避免将未保存结果标记为完成。 |
| 任务超时 | 标记相应任务失败，并允许按重试规则处理。 |
| 用户取消 | 主任务进入 cancelled，已成功结果不被覆盖。 |

---

## 12. 重试规则

- 上传失败可重新上传。
- 单个分析模块可以单独重试。
- feedback_generation 可以单独重试。
- 重试时不得覆盖已成功结果。
- 第一版设置有限重试次数，防止无限循环。
- 具体重试次数在后续工程配置中确定。

---

## 13. APP 进度展示

用户端阶段文字统一使用：

- 正在上传练习文件
- 正在检查录音和画面
- 正在分析音准和节奏
- 正在分析姿势和运弓
- 正在整理练习建议
- 分析完成

后端可以返回：

```json
{
  "taskStatus": "processing",
  "progress": 58,
  "currentStage": "motion_analysis",
  "displayText": "正在分析姿势和运弓"
}
```

进度建议：

| 阶段 | 进度 |
|---|---|
| 创建任务 | 5% |
| 上传文件 | 5%—25% |
| 媒体预处理 | 25%—35% |
| 并行分析 | 35%—80% |
| 结果汇总 | 80%—90% |
| 反馈生成 | 90%—98% |
| 完成 | 100% |

第一版允许使用估算进度，不要求精确计算。

---

## 14. APP 退出后的行为

- 用户提交分析后，任务在后端继续运行。
- 用户关闭 APP 不应取消任务。
- 用户重新进入 APP 后，可查询任务状态。
- 已完成则直接进入结果页。
- 失败则展示失败原因和重试入口。
- 未完成则继续显示分析中页面。

---

## 15. 扩展预留

未来可以增加：

- vibrato_analysis
- left_hand_fingering_analysis
- expression_analysis
- hardware_sensor_analysis

新增模块应采用新的子任务扩展，不修改现有主任务架构。

---

## 16. 当前冻结结论

本阶段 Freeze：

- 异步任务架构
- 主任务与子任务
- 媒体预处理
- 四模块并行分析
- 结果汇总
- 独立反馈生成
- 部分成功
- 单模块重试
- APP 退出后任务继续执行
- 状态机

---

## 17. AI Pipeline 分层设计

### 17.1 Pipeline 总体结构

```text
APP 数据采集
↓
媒体接入层
↓
媒体预处理层
↓
曲谱基准数据层
↓
音频分析 Pipeline
↓
视频分析 Pipeline
↓
结果标准化层
↓
结果汇总层
↓
AI 反馈生成层
↓
结果存储
↓
APP 展示
```

### 17.2 APP 数据采集层

输入：

- scoreId
- userId
- learningStage
- audioFile
- videoFile
- videoOrientation
- durationSeconds

输出：

统一 Practice Session 数据。

### 17.3 媒体接入层

FastAPI 负责：

- 用户权限校验
- 创建 practice_session
- 创建 practice_media
- 创建 AI Master Task
- 投递异步任务

### 17.4 媒体预处理层

音频：

- 格式统一
- 提取
- 降噪
- 采样率统一
- 声道统一

视频：

- 方向修正
- 抽帧
- 帧率统一
- 完整性检查
- 时间轴同步

### 17.5 曲谱基准数据层

第一版所有内置曲谱必须提前生成结构化标准数据。

例如：

- Tempo
- Time Signature
- MIDI
- Note Timeline
- Measure
- Beat

第一版不实时解析任意曲谱图片。

### 17.6 音频分析 Pipeline

音准：

```text
Audio
↓
Pitch Detection
↓
Note Segmentation
↓
Timeline Alignment
↓
Pitch Evaluation
↓
Pitch Result
```

节奏：

```text
Audio
↓
Onset Detection
↓
Timeline Alignment
↓
Rhythm Evaluation
↓
Rhythm Result
```

### 17.7 视频分析 Pipeline

姿势：

```text
Video
↓
Body Keypoints
↓
Hand Keypoints
↓
Rule Evaluation
↓
Posture Result
```

运弓：

```text
Video
↓
Bow Tracking
↓
Trajectory Analysis
↓
Rule Evaluation
↓
Bowing Result
```

### 17.8 统一输出协议

所有 AI 模块必须输出统一 JSON。

至少包括：

- module
- status
- score
- rating
- confidence
- issues
- warnings
- rawResult

以后所有 AI 模型必须遵守统一输出协议。

### 17.9 LLM 使用原则

GPT（或其他 LLM）不能直接分析音视频。

LLM 仅负责：

- 总结
- 解释
- 鼓励
- 练习建议

输入必须来自结构化分析结果。

### 17.10 Pipeline Freeze

本阶段冻结结论：

- Pipeline 分层
- 曲谱基准数据层
- 音频 Pipeline
- 视频 Pipeline
- 标准输出协议
- LLM 职责
- Confidence 输出
- Warning 输出

---

## 18. API 协议规范

### 18.1 URL 规范

统一使用：

- 小写
- 英文
- 复数资源
- kebab-case

例如：

- practice-sessions
- analysis-tasks

### 18.2 HTTP Method

统一使用：

| Method | 用途 |
|---|---|
| GET | 查询资源或资源列表。 |
| POST | 创建资源、提交操作或触发任务。 |
| PUT | 完整替换资源。 |
| PATCH | 部分更新资源。 |
| DELETE | 删除资源。 |

### 18.3 返回格式

成功：

```json
{
  "success": true,
  "code": 0,
  "message": "OK",
  "data": {}
}
```

失败：

```json
{
  "success": false,
  "code": 4001,
  "message": "错误信息",
  "data": null
}
```

### 18.4 分页规范

请求：

`?page=1&pageSize=20`

返回包含：

- items
- page
- pageSize
- total
- totalPages

### 18.5 时间规范

统一使用：

- UTC
- ISO8601

例如：

`2026-07-12T09:35:21Z`

### 18.6 UUID

所有资源主键统一使用：

UUID。

### 18.7 JSON 命名

JSON 统一使用：

camelCase。

数据库统一使用：

snake_case。

### 18.8 文件上传

统一使用：

`multipart/form-data`。

### 18.9 AI 查询方式

第一版采用轮询：

```text
GET
/api/v1/analysis/tasks/{taskId}
```

暂不采用：

- WebSocket

### 18.10 Bearer Token

统一使用：

`Authorization: Bearer Token`

### 18.11 错误码规范

统一错误码如下：

| 错误码 | 含义 |
|---|---|
| 0 | 成功 |
| 4001 | 参数错误 |
| 4010 | Token 无效或已过期 |
| 4030 | 无权限访问资源 |
| 4101 | 上传失败 |
| 4102 | 媒体错误 |
| 5001 | AI 分析失败 |
| 5000 | 系统异常 |

### 18.12 Phase 6.1 Freeze

本阶段冻结：

- RESTful API
- API Version
- URL Naming
- HTTP Method
- JSON Format
- Pagination
- Error Code
- UUID
- UTC Time
- Upload Protocol
- Polling Strategy
- Bearer Token
- JSON camelCase
- DB snake_case

---

## 19. 业务资源 API 设计

### 19.1 资源 API 原则

- API 围绕业务资源设计。
- 页面不作为 API 的一级资源。
- APP 页面可以组合多个资源 API。
- 第一阶段不设计首页专用 API、练习页专用 API 或结果页专用 API。
- 后续新增终端时优先复用现有资源接口。

### 19.2 Auth 与 Users 的职责边界

Auth 负责：

- 注册
- 登录
- 登出
- Token 刷新
- 当前登录身份
- 密码重置

Users 负责：

- 用户资料
- 昵称
- 头像
- 学习阶段
- 练习目标
- 密码修改
- 后续手机号、邮箱修改

账号认证信息与学习资料分离管理。

### 19.3 Auth API 设计

统一前缀：

`/api/v1/auth`

#### 19.3.1 注册

`POST /api/v1/auth/register`

请求字段：

```json
{
  "phone": "13800138000",
  "email": null,
  "password": "用户输入的密码"
}
```

规则：

- phone 和 email 至少有一个。
- 手机号和邮箱不能都为空。
- 手机号或邮箱不得重复注册。
- 密码不得明文保存。
- 后端仅保存 password_hash。
- 注册成功后直接返回登录凭证，无需再次登录。

返回数据至少包括：

- user
- accessToken
- refreshToken
- expiresIn

#### 19.3.2 登录

`POST /api/v1/auth/login`

请求：

```json
{
  "account": "13800138000",
  "password": "用户输入的密码"
}
```

account 可以是手机号或邮箱。

返回至少包括：

- user
- accessToken
- refreshToken
- expiresIn

登录失败统一提示：

账号或密码错误

不得通过错误信息明确暴露账号是否存在。

#### 19.3.3 刷新 Token

`POST /api/v1/auth/refresh-token`

请求：

```json
{
  "refreshToken": "xxx"
}
```

返回：

- accessToken
- refreshToken
- expiresIn

第一版使用 Access Token + Refresh Token。Access Token 有效期较短，Refresh Token 有效期较长；刷新时建议轮换 Refresh Token。具体有效期后续由工程配置确定，本阶段不写死。

#### 19.3.4 退出登录

`POST /api/v1/auth/logout`

规则：

- 使当前 Refresh Token 失效。
- APP 删除本地 Token。
- 退出后不能继续通过旧 Refresh Token 续期。

#### 19.3.5 查询当前登录身份

`GET /api/v1/auth/me`

用途：

- APP 启动时检查登录状态。
- 获取当前账号状态。
- 判断用户是否已完成学习资料。
- 判断是否需要进入学习阶段选择页。

返回至少包括：

- id
- phone
- email
- status
- profileCompleted

#### 19.3.6 发起密码重置

`POST /api/v1/auth/password-reset/request`

输入手机号或邮箱。

正式上线前接入短信或邮件服务；本地开发阶段允许使用模拟验证码。

#### 19.3.7 确认密码重置

`POST /api/v1/auth/password-reset/confirm`

请求至少包括：

- resetToken 或 verificationCode
- newPassword

### 19.4 Users API 设计

统一前缀：

`/api/v1/users`

第一版只允许用户访问和修改本人数据。

#### 19.4.1 查询我的资料

`GET /api/v1/users/me`

返回至少包括：

- id
- nickname
- avatarUrl
- learningStage
- practiceGoal
- totalPracticeDays
- totalPracticeSeconds
- createdAt
- updatedAt

该接口可以组合 users、user_profiles 两张表的数据。前端无需知道数据库内部表结构。

#### 19.4.2 修改我的普通资料

`PATCH /api/v1/users/me`

允许修改：

- nickname
- avatarUrl
- learningStage
- practiceGoal

不允许修改：

- id
- phone
- email
- password
- status
- totalPracticeDays
- totalPracticeSeconds

请求示例：

```json
{
  "nickname": "茂",
  "learningStage": "beginner",
  "practiceGoal": "每天练习20分钟"
}
```

#### 19.4.3 学习阶段处理

第一版不新增：

`PATCH /api/v1/users/learning-stage`

学习阶段直接通过：

`PATCH /api/v1/users/me`

修改。

现有资源接口已经可以承载，避免重复接口和需求扩张。

#### 19.4.4 修改密码

`PATCH /api/v1/users/me/password`

请求：

```json
{
  "currentPassword": "旧密码",
  "newPassword": "新密码"
}
```

规则：

- 必须验证当前密码。
- 修改成功后旧 Refresh Token 失效。
- 新密码必须符合密码安全规则。

#### 19.4.5 手机号和邮箱修改

保留接口概念：

- `PATCH /api/v1/users/me/phone`
- `PATCH /api/v1/users/me/email`

当前阶段只记录接口预留，不要求 MVP 立即实现。

修改手机号、邮箱需要验证码和安全验证，不能通过普通资料接口直接修改。

### 19.5 第一版认证范围

第一版支持：

- 手机号 + 密码注册
- 邮箱 + 密码注册
- 手机号或邮箱 + 密码登录
- Access Token
- Refresh Token
- Token 刷新
- 退出登录
- 查询当前登录用户
- 密码重置
- 用户资料查询和修改

第一版暂不支持：

- 微信登录
- Apple ID 登录
- Google 登录
- 短信验证码直接登录
- 人脸登录
- 教师账号
- 机构账号
- 管理员角色体系
- 多设备管理页面

不得自行扩展上述范围。

### 19.6 认证流程

```text
用户注册或登录
↓
Auth 返回 Access Token 和 Refresh Token
↓
APP 保存登录凭证
↓
APP 携带 Access Token 调用受保护接口
↓
GET /api/v1/auth/me
↓
判断账号状态和 profileCompleted
↓
GET /api/v1/users/me
↓
如果 learningStage 为空
↓
进入学习阶段选择页
↓
PATCH /api/v1/users/me
↓
保存 learningStage
↓
进入首页
```

### 19.7 错误码

Auth 和 Users 错误码如下：

| 错误码 | 含义 |
|---|---|
| 1001 | 请求参数错误 |
| 1002 | 未登录 |
| 1003 | Access Token 已失效 |
| 1004 | 无权限 |
| 1101 | 手机号或邮箱已注册 |
| 1102 | 账号或密码错误 |
| 1103 | 账号已停用 |
| 1104 | Refresh Token 无效或已过期 |
| 1105 | 当前密码错误 |
| 1106 | 密码格式不符合要求 |
| 1107 | 验证码无效或已过期 |
| 1201 | 用户资料不存在 |
| 5000 | 系统异常 |

上述错误码不与 Phase 6.1 已定义的统一错误码冲突。

### 19.8 安全规则

- 密码必须安全哈希保存。
- 不得明文存储密码。
- Token 不得写入日志。
- 正式环境必须使用 HTTPS。
- 登录接口必须限制高频失败尝试。
- 用户只能访问和修改本人资料。
- Refresh Token 必须支持失效和撤销。
- 修改密码后旧 Refresh Token 必须失效。
- 手机号、邮箱根据使用场景进行脱敏。
- 登录错误不得暴露账号是否存在。
- 不得在 API 返回中输出 password_hash。
