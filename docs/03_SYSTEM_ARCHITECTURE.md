# 03_SYSTEM_ARCHITECTURE

## 文档信息

| 项目 | 内容 |
|---|---|
| 文档名称 | 系统架构设计 |
| 项目 | AI Violin Platform |
| 版本 | v0.1 |
| 状态 | DRAFT |
| 负责人 | ChatGPT |
| 创建日期 | 2026-07-11 |

---

## 1. 系统目标

AI Violin Platform 第一阶段系统目标是支持：

- 用户学习
- 曲谱浏览
- 练习录制
- AI 分析
- AI 反馈
- 练习记录
- 后续硬件扩展

系统设计应简单、清晰、可扩展。

---

## 2. 总体架构

第一阶段建议采用：

```text
APP 前端（手机端）
↓
后端 API 服务
↓
业务数据库
↓
AI 分析服务
↓
文件存储 / 音视频存储
```

其中：

- 前端负责用户界面和交互
- 后端负责业务逻辑和数据管理
- AI 服务负责音频、视频、曲谱分析
- 数据库存储用户、曲谱、练习记录
- 文件存储保存音频、视频和相关文件

---

## 3. 前端模块

前端主要模块：

- 登录注册页面
- 首页
- 曲谱列表页
- 曲谱详情页
- 练习录制页
- AI 分析结果页
- 练习记录页
- 个人中心页
- 设置页

前端应优先适配手机端。

---

## 4. 后端模块

后端主要模块：

- 用户管理
- 曲谱管理
- 练习任务管理
- 音视频文件管理
- AI 分析任务管理
- 练习结果管理
- 反馈生成管理
- 权限与安全管理

---

## 5. AI 服务模块

AI 服务可拆分为：

- 音频分析服务
- 视频姿势识别服务
- 运弓分析服务
- 曲谱解析服务
- AI 反馈生成服务

第一阶段实现音频分析、AI 反馈、姿势识别和运弓分析；视频类能力以基础反馈为目标，后续持续提升精度。

AI 服务进一步划分：

- Media Service
- Audio Analysis Service
- Video Analysis Service
- Aggregation Service
- Feedback Service

AI 服务采用流水线（Pipeline）架构。以后新增分析模块时，只新增 Pipeline，不修改总体架构。

---

## 6. 数据库模块

数据库主要存储：

- 用户信息
- 曲谱信息
- 练习记录
- AI 分析结果
- AI 反馈内容
- 学习进度
- 系统配置

---

## 7. 文件存储

需要存储的文件包括：

- 用户练习录音
- 用户练习视频
- 曲谱文件
- 示范音频
- 示范视频
- AI 分析中间文件

第一阶段先使用本地或服务器存储，后续再接云存储。

---

## 8. AI 分析流程

AI 分析采用异步任务。主任务负责调度，子任务负责具体分析；支持部分成功和单模块重试，用户关闭 APP 后任务继续执行。

基本流程：

```text
用户结束练习
↓
APP 完成音频和视频录制
↓
上传媒体文件
↓
后端创建 AI 主任务
↓
媒体预处理
↓
并行执行音准、节奏、姿势、运弓分析
↓
汇总结构化结果
↓
生成自然语言反馈
↓
保存 AI 结果和练习记录
↓
APP 查询并展示结果
```

---

## 9. 硬件扩展预留

虽然第一阶段不做智能硬件，但系统应预留硬件扩展思路。

未来可能接入：

- 智能指板
- 智能琴弓
- 智能小提琴
- 蓝牙传感器
- MIDI 或自定义协议设备

后续可增加：

- 设备管理模块
- 设备绑定
- 设备数据上传
- 硬件固件版本
- 传感器数据分析
- 软硬件联合反馈

---

## 10. 初步技术建议

第一阶段产品形态为 APP，前端优先适配手机端。

第一阶段推荐技术栈：

- APP 前端：React Native + Expo
- 后端 API：Python + FastAPI
- 数据库：PostgreSQL
- AI 分析服务：Python
- 文件存储：第一阶段先使用本地或服务器存储，后续再接云存储
- 部署：先本地开发和测试，后续部署到云服务器

---

## 11. API 架构规范

### 11.1 API 风格

第一阶段采用：

- RESTful API

第一阶段不采用：

- GraphQL
- gRPC
- SOAP

### 11.2 API 版本

统一使用：

`/api/v1/`

以后升级使用：

`/api/v2/`

### 11.3 API 分组

统一采用：

- /auth
- /users
- /scores
- /practice-sessions
- /practice-media
- /analysis/tasks
- /analysis/results
- /analysis/feedbacks

每组保持职责单一。

### 11.4 资源 API 原则

系统 API 采用资源导向设计，不采用页面导向设计。

API 围绕以下业务资源组织：

- auth
- users
- scores
- practice-sessions
- practice-media
- analysis-tasks
- analysis-results
- analysis-feedbacks

页面根据业务需要组合多个资源接口。

例如，练习记录详情页可以组合：

- practice-session
- practice-media
- analysis-result
- analysis-feedback

不得为每个页面单独设计：

- home-api
- practice-page-api
- result-page-api

采用资源 API 的原因：

- 页面会变化，资源相对稳定。
- APP、Web 后台、教师端、家长端和未来硬件可以复用。
- 降低重复开发。
- 避免接口随页面增加而失控。
- 与 RESTful API 规范一致。

资源 API 是 Phase 6.2 已确认架构原则。

### 11.5 曲谱服务与基准数据

- Scores Service 负责曲谱主信息。
- Score Reference Service 或 Scores Service 内部基准模块负责读取 score_notes。
- APP 读取曲谱展示数据。
- AI Service 通过后端读取正式音符基准数据。
- 客户端上传的音符基准不得作为可信标准答案。

保持现有总体架构不变，不新增新的系统一级模块。

### 11.6 Practice Session 服务职责

Practice Session Service 负责：

- 练习创建
- 练习生命周期
- 状态转换
- 历史查询
- 权限校验
- 幂等控制
- 软删除

- Practice Session Service 不处理实际音视频文件。
- Practice Media Service 处理文件。
- Analysis Task Service 处理 AI 分析任务。
- 三者通过 practiceSessionId 关联。

不新增新的系统一级架构层，只补充职责说明。
