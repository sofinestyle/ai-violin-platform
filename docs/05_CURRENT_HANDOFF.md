# 05_CURRENT_HANDOFF

## 文档信息

| 项目 | 内容 |
|---|---|
| 文档名称 | 当前交接记录 |
| 项目 | AI Violin Platform |
| 版本 | v0.1 |
| 状态 | DRAFT |
| 负责人 | ChatGPT |
| 创建日期 | 2026-07-11 |

---

## 1. 当前项目阶段

当前处于：

> Phase 7.1：AI 模块公共规范与运行契约已完成（待 Freeze）

---

## 2. 已确认事项

### 2.1 项目定位

AI Violin Platform 是面向中文用户的小提琴 AI 学习与陪练平台。

第一阶段采用：

> 传统小提琴 + 手机 + AI

不依赖智能硬件。

### 2.2 当前已确认 MVP 决策

1. 第一版产品形态为 APP。
2. 第一版面向中文用户。
3. MVP 包含姿势识别。
4. MVP 包含运弓分析。
5. 技术栈采用 React Native + Expo、Python + FastAPI、PostgreSQL、Python AI 服务。
6. 第一版使用少量内置曲谱验证。

已确认 MVP 用户流程和 APP 页面结构，详见：
docs/06_MVP_USER_FLOW.md

已确认 APP 横竖屏策略：

- APP 默认竖屏
- 普通页面竖屏
- 练习录制页横屏优先
- AI 分析结果页和练习记录页竖屏
- 横屏练习页必须处理视频方向、摄像头预览和取景提示

已确认 APP 信息架构调整：

- 第一版 APP 采用 4 个底部 Tab：首页、曲谱、练习记录、我的
- MVP 页面由原 8 个核心页面调整为 12 个页面
- 新增练习准备页、AI 分析中页面、练习记录详情页
- 核心练习路径为：曲谱列表 → 曲谱详情 → 练习准备 → 横屏练习录制 → AI 分析中 → AI 分析结果 → 练习记录详情

已完成数据库初版结构设计，详见：
docs/07_DATABASE_SCHEMA.md

数据库第一版采用：

- PostgreSQL
- 9 张核心表
- 结构化字段 + JSONB 扩展字段
- 支撑用户、曲谱、练习记录、音视频文件、AI 分析任务、AI 分析结果和 AI 反馈

### 2.3 Phase 5.1 已完成内容

- AI 异步任务架构
- Master Task / Sub Task
- 媒体预处理流程
- 四模块并行分析
- AI 任务状态机
- Practice Session 状态机
- 部分成功策略
- 单模块重试策略
- APP 分析进度展示
- APP 退出后后台继续分析
- 项目永久工作流

### 2.4 Phase 5.2 已完成内容

- AI Pipeline
- 曲谱基准数据层
- Pipeline 分层
- AI 输出协议
- LLM 职责

### 2.5 Phase 6.1 已完成内容

- API Specification
- REST API
- URL 命名
- API Version
- JSON 返回格式
- 分页规范
- 错误码
- UUID
- Token
- 上传规范
- Polling

### 2.6 Phase 6.2 第一部分已完成内容

- 资源 API 原则
- Auth 与 Users 职责分离
- Access Token + Refresh Token
- 注册接口
- 登录接口
- Token 刷新接口
- 退出登录接口
- 当前登录用户接口
- 密码重置接口
- 用户资料查询接口
- 用户资料修改接口
- 密码修改接口
- 认证错误码
- 认证安全规则

### 2.7 Phase 6.2 第二部分已完成内容

- Scores 资源 API
- 曲谱列表接口
- 曲谱详情接口
- 曲谱基准数据接口
- 曲谱分页及基础筛选
- 普通用户只读权限
- 第一版不设计推荐专用接口
- 第一版不设计分类、标签和曲集
- 独立 score_notes 表
- 曲谱基准数据由后端提供
- 第一版 9 张核心表

### 2.8 Phase 6.2 第三部分之一已完成内容

- Practice Session 资源职责
- 练习生命周期
- 创建练习接口
- 结束录制接口
- 取消录制软删除
- 历史练习列表
- 单次练习详情
- 软删除规则
- 状态转换规则
- 幂等规则
- 重复请求处理
- Practice Sessions 错误码

### 2.9 Phase 6.2 第三部分之二已完成内容

- Practice Media 资源职责
- multipart/form-data 上传
- 音频和视频统一上传接口
- 媒体状态查询接口
- 按 Practice Session 查询媒体接口
- 音视频格式规范
- 文件大小与时长限制原则
- 基础媒体质量校验
- 部分媒体可用策略
- Practice Session 状态联动
- 重传和替换规则
- 文件访问安全
- Practice Media 错误码

### 2.10 Phase 6.2 第四部分已完成内容

- AI 分析任务创建接口
- AI 任务状态查询接口
- 按 Practice Session 查询任务
- 轮询规则
- 单模块重试
- Analysis Result 结构
- 统一问题项结构
- 置信度与警告规则
- 模块评分和综合评价原则
- Feedback 查询接口
- LLM 职责边界
- Feedback 独立失败策略
- 按 Practice Session 查询结果和反馈
- Analysis 权限规则
- Analysis 错误码

### 2.11 Phase 6.3 已完成内容

- API 与数据库一致性
- JSON 结构统一
- Analysis Results 重构
- Feedback 数据结构统一
- 最终错误码规范
- MVP 最小数据库字段确认

### 2.12 Phase 7.1 已完成内容

- AI Module Contract
- Module Boundary
- Module Input
- Module Output
- Score / Confidence
- Issue / Warning
- Runtime Metadata

### 2.13 长期目标

项目长期目标不是单一软件，而是构建 AI Music Platform。

未来形成：

- 软件平台
- 智能硬件
- 教学内容
- 云服务

四位一体生态。

---

### 2.14 工作方式

项目采用文档驱动开发：

```text
产品讨论
↓
SPEC 文档更新
↓
文档 Freeze
↓
Codex 开发
↓
测试
↓
更新 SPEC
```

---

### 2.15 角色分工

项目经理：用户本人。

ChatGPT 承担：

- 产品经理
- 系统架构师
- AI 架构设计师
- 数据库设计师
- UI/UX 设计师
- 技术路线规划

Codex 承担：

- 前端开发
- 后端开发
- 数据库开发
- API 开发
- Bug 修复
- 重构

---

### 2.16 文档管理方式

由于 ChatGPT 对话会被自动压缩，因此后续以 GitHub 中的 Markdown 文档作为项目长期记忆。

第一阶段采用：

```text
ChatGPT 生成文档内容
↓
用户复制给 Codex
↓
Codex 更新 GitHub 文档
↓
后续以 GitHub 文档为准
```

---

## 3. 当前已创建文档

建议第一批创建：

- README.md
- docs/00_PROJECT_GOVERNANCE.md
- docs/00_PROJECT_WORKFLOW.md
- docs/01_PROJECT_OVERVIEW.md
- docs/02_PRODUCT_SPEC.md
- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md
- docs/06_MVP_USER_FLOW.md
- docs/07_DATABASE_SCHEMA.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md
- CHANGELOG.md

---

## 4. 下一阶段

下一阶段为：

> Phase 7.2：媒体预处理模块详细设计

Phase 7.1 已完成 AI 模块公共规范与运行契约同步，待 Freeze 后进入媒体预处理模块详细设计。

---

## 5. 待决策问题

以下 6 项已确认，详见“2.2 当前已确认 MVP 决策”：

1. 第一版产品形态
2. 第一版目标用户
3. MVP 姿势识别范围
4. MVP 运弓分析范围
5. 第一阶段技术栈
6. 第一版曲谱验证方式

---

## 6. 给下次 ChatGPT 会话的提示

继续项目之前，必须依次读取：

- README.md
- docs/00_PROJECT_GOVERNANCE.md
- docs/00_PROJECT_WORKFLOW.md
- docs/05_CURRENT_HANDOFF.md
- 当前阶段相关文档

所有后续阶段必须遵守 docs/00_PROJECT_WORKFLOW.md。

---

## 7. 当前对话交接

交接日期：2026-07-12。

### 7.1 当前已完成工作

- 已建立 GitHub Markdown 唯一事实来源和项目永久工作流。
- 已确认并同步第一阶段 MVP、APP 页面结构、横竖屏策略和用户流程。
- 已完成 9 张核心表的数据库设计及 Phase 6.3 API 与数据库一致性核对。
- 已完成 AI 异步任务、AI Pipeline、统一结构化结果和 LLM 职责边界设计。
- 已完成 API 通用规范及 Auth、Users、Scores、Practice Sessions、Practice Media、Analysis Tasks、Analysis Results、Analysis Feedbacks API 设计。
- 已统一项目错误码范围，并完成 Phase 6 全部文档 Freeze。
- 已完成 Phase 7.1 AI 模块公共规范与运行契约，包括模块边界、统一输入输出、Score / Confidence、Issue / Warning 和 Runtime Metadata。
- 交接前所有修改均已 Commit，并已 Push 到远程 main 分支。

### 7.2 本次对话已修改文件

- CHANGELOG.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md
- docs/08_AI_ANALYSIS_TASK_FLOW.md

未修改数据库、API 和业务代码。

### 7.3 Git 状态

- 交接更新前最新 Commit SHA：5ae37c50aa015d8bfab9629e5159e5ffee20fbbc
- 交接更新前本地 main 与 origin/main 一致。
- 本交接文档更新提交的最终 SHA 以远程 main 最新 HEAD 为准。

### 7.4 当前阶段状态

> Phase 7.1：AI 模块公共规范与运行契约已完成（待 Freeze）

当前暂停继续开发，等待项目经理 Freeze 后进入下一阶段。

### 7.5 下一步建议

新对话应先按以下顺序读取：

1. README.md
2. docs/00_PROJECT_GOVERNANCE.md
3. docs/00_PROJECT_WORKFLOW.md
4. docs/05_CURRENT_HANDOFF.md
5. CHANGELOG.md
6. docs/04_AI_TECHNICAL_ROUTE.md
7. docs/07_DATABASE_SCHEMA.md
8. docs/08_AI_ANALYSIS_TASK_FLOW.md

确认同步并 Freeze 后，从 Phase 7.2：媒体预处理模块详细设计继续。

### 7.6 未解决问题

当前没有阻塞交接的问题。

以下数据库字段仍为已记录的待确认项，不在本阶段加入：

- practice_sessions.deleted_at
- practice_sessions.idempotency_key
- practice_media.audio_size_bytes
- practice_media.video_size_bytes
- practice_media.checksum

这些待确认项应在实际工程实现确有需要时，再按项目工作流提出变更并确认。
