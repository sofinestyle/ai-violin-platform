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

> 项目初始化与文档体系建立阶段

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
- 8 张核心表
- 结构化字段 + JSONB 扩展字段
- 支撑用户、曲谱、练习记录、音视频文件、AI 分析任务、AI 分析结果和 AI 反馈

### 2.3 长期目标

项目长期目标不是单一软件，而是构建 AI Music Platform。

未来形成：

- 软件平台
- 智能硬件
- 教学内容
- 云服务

四位一体生态。

---

### 2.4 工作方式

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

### 2.5 角色分工

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

### 2.6 文档管理方式

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
- docs/01_PROJECT_OVERVIEW.md
- docs/02_PRODUCT_SPEC.md
- docs/03_SYSTEM_ARCHITECTURE.md
- docs/04_AI_TECHNICAL_ROUTE.md
- docs/05_CURRENT_HANDOFF.md
- docs/06_MVP_USER_FLOW.md
- docs/07_DATABASE_SCHEMA.md

---

## 4. 下一步建议

下一步建议：

1. 设计 AI 分析任务流程
2. 编写第一版可开发的 FEATURE SPEC
3. 后续进入 UI/UX 页面原型设计
4. 后续细化 API 设计

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

下次继续本项目时，可以先发送：

```text
请先读取 GitHub 中 AI Violin Platform 的项目文档，再继续。
```

如果 GitHub 尚未建立，可以发送：

```text
请基于当前第一版项目 MD 文档，继续完善 AI Violin Platform。
```
