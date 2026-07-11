# 07_DATABASE_SCHEMA

## 文档信息

| 项目 | 内容 |
|---|---|
| 文档名称 | 数据库初版结构设计 |
| 项目 | AI Violin Platform |
| 版本 | v0.1 |
| 状态 | DRAFT |
| 负责人 | ChatGPT |
| 审批人 | 项目经理 |
| 创建日期 | 2026-07-11 |

---

## 1. 文档目的

本文档用于定义 AI Violin Platform 第一阶段 MVP 的数据库初版结构。

第一阶段数据库目标是支撑完整学习闭环：

用户 → 曲谱 → 练习录制 → 音视频文件 → AI 分析任务 → AI 分析结果 → AI 反馈 → 练习记录。

---

## 2. 数据库设计原则

第一版数据库遵循以下原则：

1. 支撑 MVP 闭环，不做过度设计。
2. 优先保证用户、曲谱、练习记录、AI 分析结果可完整保存。
3. 音准、节奏、姿势、运弓等 AI 详细结果可先使用 JSONB 字段保存，方便后续扩展。
4. 练习录制页采用横屏，因此练习视频必须记录方向信息。
5. 数据库采用 PostgreSQL。
6. 后续如接入智能指板、智能琴弓、智能小提琴，可在现有结构上扩展设备相关表。

---

## 3. 核心数据关系

一个用户可以产生多次练习记录。

一首曲谱可以被多个用户练习。

一次练习记录对应：

1. 一份音频 / 视频文件
2. 一个 AI 分析任务
3. 一份 AI 分析结果
4. 一份 AI 练习反馈

核心关系如下：

用户 → 练习记录 → 练习媒体 → AI 分析任务 → AI 分析结果 → AI 反馈

曲谱 → 练习记录

---

## 4. 第一版核心表

第一版建议设计 8 张核心表：

| 表名 | 作用 |
|---|---|
| users | 用户账号基础信息 |
| user_profiles | 用户学习资料 |
| scores | 内置曲谱 |
| practice_sessions | 练习记录 |
| practice_media | 练习音频和视频文件 |
| ai_analysis_tasks | AI 分析任务 |
| ai_analysis_results | AI 结构化分析结果 |
| ai_feedbacks | AI 自然语言反馈 |

---

## 5. users 用户表

### 5.1 表作用

保存用户账号基础信息。

### 5.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 用户 ID |
| email | VARCHAR | 邮箱，可为空 |
| phone | VARCHAR | 手机号，可为空 |
| password_hash | VARCHAR | 加密后的密码 |
| status | VARCHAR | 账号状态 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 5.3 status 建议值

| 值 | 含义 |
|---|---|
| active | 正常 |
| disabled | 停用 |
| deleted | 已删除 |

---

## 6. user_profiles 用户资料表

### 6.1 表作用

保存用户学习资料和练习偏好。

### 6.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 资料 ID |
| user_id | UUID | 关联 users.id |
| nickname | VARCHAR | 昵称 |
| learning_stage | VARCHAR | 学习阶段 |
| practice_goal | VARCHAR | 练习目标 |
| total_practice_days | INTEGER | 累计练习天数 |
| total_practice_seconds | INTEGER | 累计练习时长，单位秒 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 6.3 learning_stage 建议值

| 值 | 含义 |
|---|---|
| zero_based | 零基础 |
| beginner | 初学入门 |
| basic | 有一定基础 |
| advanced | 进阶练习 |

---

## 7. scores 曲谱表

### 7.1 表作用

保存第一版内置曲谱信息。

第一版曲谱数量不宜过多，先用于验证学习闭环。

### 7.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 曲谱 ID |
| title | VARCHAR | 曲谱名称 |
| difficulty | VARCHAR | 难度 |
| suitable_stage | VARCHAR | 适合学习阶段 |
| estimated_seconds | INTEGER | 预计练习时长，单位秒 |
| score_file_url | TEXT | 曲谱文件地址 |
| demo_audio_url | TEXT | 示范音频地址 |
| description | TEXT | 曲谱说明 |
| is_active | BOOLEAN | 是否启用 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 7.3 difficulty 建议值

| 值 | 含义 |
|---|---|
| easy | 简单 |
| normal | 普通 |
| hard | 较难 |

### 7.4 第一版建议内置曲谱

| 曲谱 | 说明 |
|---|---|
| 小星星 | 初学曲目 |
| 生日快乐 | 简单旋律 |
| 欢乐颂 | 入门练习 |
| 简单音阶练习 | 音准练习 |
| 空弦练习 | 基础运弓练习 |

---

## 8. practice_sessions 练习记录表

### 8.1 表作用

保存用户每一次练习的主记录。

### 8.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 练习 ID |
| user_id | UUID | 关联 users.id |
| score_id | UUID | 关联 scores.id |
| started_at | TIMESTAMP | 练习开始时间 |
| ended_at | TIMESTAMP | 练习结束时间 |
| duration_seconds | INTEGER | 练习时长，单位秒 |
| overall_rating | VARCHAR | 总评 |
| status | VARCHAR | 练习记录状态 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 8.3 overall_rating 建议值

| 值 | 含义 |
|---|---|
| excellent | 优秀 |
| good | 良好 |
| needs_improvement | 需要改进 |

### 8.4 status 建议值

| 值 | 含义 |
|---|---|
| recording | 录制中 |
| submitted | 已提交分析 |
| analyzing | 分析中 |
| analyzed | 已完成分析 |
| failed | 分析失败 |
| deleted | 已删除 |

---

## 9. practice_media 练习媒体表

### 9.1 表作用

保存一次练习对应的音频、视频文件信息。

### 9.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 媒体 ID |
| practice_session_id | UUID | 关联 practice_sessions.id |
| audio_file_url | TEXT | 音频文件地址 |
| video_file_url | TEXT | 视频文件地址 |
| video_orientation | VARCHAR | 视频方向 |
| video_width | INTEGER | 视频宽度 |
| video_height | INTEGER | 视频高度 |
| duration_seconds | INTEGER | 媒体时长 |
| file_status | VARCHAR | 文件状态 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 9.3 video_orientation 建议值

| 值 | 含义 |
|---|---|
| portrait | 竖屏 |
| landscape_left | 横屏，左横向 |
| landscape_right | 横屏，右横向 |
| unknown | 未知 |

### 9.4 file_status 建议值

| 值 | 含义 |
|---|---|
| uploading | 上传中 |
| uploaded | 已上传 |
| processing | 处理中 |
| ready | 可分析 |
| failed | 失败 |

---

## 10. ai_analysis_tasks AI 分析任务表

### 10.1 表作用

保存 AI 分析任务的执行过程和状态。

### 10.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 任务 ID |
| practice_session_id | UUID | 关联 practice_sessions.id |
| task_status | VARCHAR | 任务状态 |
| task_type | VARCHAR | 任务类型 |
| started_at | TIMESTAMP | 开始时间 |
| finished_at | TIMESTAMP | 结束时间 |
| error_message | TEXT | 失败原因 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 10.3 task_status 建议值

| 值 | 含义 |
|---|---|
| pending | 待处理 |
| uploading | 上传中 |
| processing | 分析中 |
| success | 成功 |
| failed | 失败 |

### 10.4 task_type 建议值

| 值 | 含义 |
|---|---|
| full_practice_analysis | 完整练习分析 |
| pitch_analysis | 音准分析 |
| rhythm_analysis | 节奏分析 |
| posture_analysis | 姿势识别 |
| bowing_analysis | 运弓分析 |

---

## 11. ai_analysis_results AI 分析结果表

### 11.1 表作用

保存 AI 输出的结构化分析结果。

### 11.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 结果 ID |
| practice_session_id | UUID | 关联 practice_sessions.id |
| task_id | UUID | 关联 ai_analysis_tasks.id |
| pitch_score | INTEGER | 音准评分 |
| rhythm_score | INTEGER | 节奏评分 |
| posture_score | INTEGER | 姿势评分 |
| bowing_score | INTEGER | 运弓评分 |
| overall_score | INTEGER | 综合评分 |
| pitch_issues | JSONB | 音准问题 |
| rhythm_issues | JSONB | 节奏问题 |
| posture_issues | JSONB | 姿势问题 |
| bowing_issues | JSONB | 运弓问题 |
| raw_result | JSONB | AI 原始结果 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 11.3 评分说明

第一版评分建议使用 0 到 100 分。

前端展示不一定直接显示数字，可转换为：

| 分数范围 | 前端展示 |
|---|---|
| 85-100 | 优秀 |
| 65-84 | 良好 |
| 0-64 | 需要改进 |

---

## 12. ai_feedbacks AI 反馈表

### 12.1 表作用

保存 AI 生成的自然语言练习反馈。

### 12.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 反馈 ID |
| practice_session_id | UUID | 关联 practice_sessions.id |
| analysis_result_id | UUID | 关联 ai_analysis_results.id |
| summary_text | TEXT | 本次练习总评 |
| main_issues | TEXT | 主要问题 |
| improvement_advice | TEXT | 改进建议 |
| next_practice_focus | TEXT | 下次练习重点 |
| encouragement_text | TEXT | 鼓励性反馈 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

---

## 13. 数据写入流程示例

用户完成一次练习后，数据写入流程如下：

1. 用户选择曲谱。
2. 系统创建 practice_sessions 记录，状态为 recording。
3. 用户完成录音和录像。
4. 系统创建 practice_media 记录。
5. 用户提交 AI 分析。
6. 系统创建 ai_analysis_tasks 记录，状态为 pending。
7. AI 服务开始分析，任务状态更新为 processing。
8. AI 分析完成，写入 ai_analysis_results。
9. AI 生成练习反馈，写入 ai_feedbacks。
10. practice_sessions.status 更新为 analyzed。
11. 用户可在练习记录详情页查看本次练习结果。

---

## 14. 第一版暂不设计的数据表

第一版暂不设计：

- 教师表
- 机构表
- 课程表
- 会员表
- 订单表
- 支付表
- 社区帖子表
- 评论表
- 智能硬件设备表
- 硬件传感器数据表

这些内容后续版本再扩展。

---

## 15. 当前结论

第一版数据库采用：

- PostgreSQL
- 8 张核心表
- 结构化字段 + JSONB 扩展字段
- 优先支撑 MVP 练习闭环
- 为未来 AI 分析增强和智能硬件接入预留扩展空间
