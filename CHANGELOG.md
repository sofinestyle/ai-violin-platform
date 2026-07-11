# CHANGELOG

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
