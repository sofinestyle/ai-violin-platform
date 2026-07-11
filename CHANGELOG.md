# CHANGELOG

## 2026-07-11｜Phase 6.2 第四部分

### 更新

- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md
- docs/06_MVP_USER_FLOW.md
- docs/07_DATABASE_SCHEMA.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- CHANGELOG.md

### Freeze

- POST /api/v1/analysis/tasks
- 创建任务后立即返回 taskId
- 同一 Practice Session 只保留一个有效主任务
- GET /api/v1/analysis/tasks/{taskId}
- GET /api/v1/practice-sessions/{practiceSessionId}/analysis-task
- 第一版采用轮询，不采用 WebSocket
- 第一版普通用户不能取消任务
- POST /api/v1/analysis/tasks/{taskId}/retries
- GET /api/v1/analysis/results/{resultId}
- Analysis Result 对普通用户只读
- 原始模型中间结果不返回 APP
- 问题项采用统一结构
- 低置信度结果不得作为确定错误
- 失败模块不按 0 分计入综合评价
- 综合评分由后端计算
- GET /api/v1/analysis/feedbacks/{feedbackId}
- LLM 只根据结构化结果生成反馈
- Feedback 失败不影响结构化结果展示
- 提供按 Practice Session 查询任务、结果和反馈接口
- 所有 Analysis API 需要登录并限制为本人数据
- 数据库扩展字段暂列待确认项

## 2026-07-11｜Phase 6.2 第三部分之二

### 更新

- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md
- docs/06_MVP_USER_FLOW.md
- docs/07_DATABASE_SCHEMA.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- CHANGELOG.md

### Freeze

- Practice Media 独立负责练习音视频文件
- POST /api/v1/practice-media
- 第一版同一接口上传音频和视频
- 使用 multipart/form-data
- 支持 m4a、aac、wav、mp4、mov
- 音频统一转换为 WAV、单声道、44.1 kHz
- 视频统一转换为 MP4、H.264
- GET /api/v1/practice-media/{mediaId}
- GET /api/v1/practice-sessions/{practiceSessionId}/media
- 上传后进行基础质量校验
- 支持部分媒体可用和部分分析
- 两类媒体均不可用时禁止正常 AI 分析
- Practice Session 与 Practice Media 状态联动
- AI 分析开始后不得替换媒体
- 第一版不开放单独删除媒体接口
- 媒体访问必须进行权限控制
- 额外媒体元数据字段暂列待确认项

## 2026-07-11｜Phase 6.2 第三部分之一

### 更新

- docs/03_SYSTEM_ARCHITECTURE.md
- docs/05_CURRENT_HANDOFF.md
- docs/06_MVP_USER_FLOW.md
- docs/07_DATABASE_SCHEMA.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- CHANGELOG.md

### Freeze

- Practice Session 为一次练习主业务记录
- POST /api/v1/practice-sessions
- 用户身份从 Token 获取
- 前端不得提交 userId
- PATCH action 完成或取消录制
- 前端不得直接修改 status
- Analysis Tasks 保持独立资源
- GET /api/v1/practice-sessions
- GET /api/v1/practice-sessions/{practiceSessionId}
- DELETE 使用软删除
- 创建后不得修改 scoreId
- 后端严格控制状态转换
- Idempotency-Key 防止重复创建
- 重复结束请求需保证幂等
- deleted_at 与 idempotency_key 暂列待确认项

## 2026-07-11｜Phase 6.2 第二部分

### 更新

- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md
- docs/07_DATABASE_SCHEMA.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- CHANGELOG.md

### Freeze

- Scores 采用资源 API
- GET /api/v1/scores
- GET /api/v1/scores/{scoreId}
- GET /api/v1/scores/{scoreId}/reference
- 支持分页、难度、学习阶段和关键词筛选
- 第一版不新增推荐专用接口
- 第一版不新增分类、标签和曲集接口
- 普通用户只能读取内置曲谱
- Scores API 第一版需要登录
- 曲谱标准答案由后端数据库提供
- 新增独立 score_notes 表
- 第一版数据库核心表由 8 张调整为 9 张
- 不采用 scores.reference_data JSONB

## 2026-07-11｜Phase 6.2 第一部分

### 更新

- docs/03_SYSTEM_ARCHITECTURE.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- docs/05_CURRENT_HANDOFF.md
- CHANGELOG.md

### Freeze

- 采用资源 API，不采用页面 API
- Auth 与 Users 分离
- Access Token + Refresh Token
- 手机号或邮箱加密码注册、登录
- 注册成功后直接返回登录凭证
- Token 刷新和退出机制
- 当前登录用户查询
- 密码重置
- 用户本人资料查询和修改
- 学习阶段通过 PATCH /api/v1/users/me 修改
- 密码使用独立接口修改
- 用户只能访问本人数据

## 2026-07-11｜Phase 6.1

### 更新

- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- docs/05_CURRENT_HANDOFF.md

### Freeze

- REST API
- API Version
- Response Format
- Pagination
- Error Code
- Token
- Upload Protocol
- Polling Strategy

## 2026-07-11｜Phase 5.2

### 更新

- docs/08_AI_ANALYSIS_TASK_FLOW.md
- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md

### Freeze

- AI Pipeline
- Pipeline Layer
- Output Protocol
- LLM Responsibility
- Confidence Strategy

## 2026-07-11｜Phase 5.1

### 新增

- docs/00_PROJECT_WORKFLOW.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- CHANGELOG.md

### 更新

- README.md
- docs/03_SYSTEM_ARCHITECTURE.md
- docs/05_CURRENT_HANDOFF.md
- docs/07_DATABASE_SCHEMA.md

### Freeze

- GitHub 作为项目唯一事实来源
- Freeze 后必须先同步 GitHub
- ChatGPT 自动生成中文 Codex 指令
- Codex 更新文档、Commit、Push 并中文反馈
- 新会话自动读取项目治理、工作流和交接文档
- 中文文档、英文代码标识
- 非必要不新增目录和文档
- AI 主任务与子任务架构
- AI 四模块并行分析
- 部分成功与单模块重试
- AI 任务状态机
