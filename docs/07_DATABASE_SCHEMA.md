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

scores
↓
score_notes

一首曲谱对应多个曲谱音符记录。

---

## 4. 第一版核心表

第一版建议设计 9 张核心表：

| 表名 | 作用 |
|---|---|
| users | 用户账号基础信息 |
| user_profiles | 用户学习资料 |
| scores | 内置曲谱 |
| score_notes | 保存内置曲谱的结构化音符和时间轴基准数据 |
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
| cover_image_url | TEXT | 曲谱封面图，可为空 |
| practice_goal | TEXT | 该曲谱的练习目标 |
| tempo | INTEGER | 默认速度，单位 BPM |
| time_signature | VARCHAR | 拍号，例如 4/4 |
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

score_file_url 保存曲谱文件地址，demo_audio_url 保存示范音频。tempo 和 time_signature 属于曲谱级基准信息；具体音符时间轴不放入 scores 表，也不新增 reference_data JSONB 字段。

### 7.5 score_notes 表

#### 表作用

保存每首内置曲谱的标准音符、节拍位置、小节位置和时值，为音准和节奏分析提供可信基准数据。

#### 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 音符记录 ID |
| score_id | UUID | 关联 scores.id |
| note_index | INTEGER | 音符在曲谱中的顺序编号 |
| pitch | VARCHAR | 音名，例如 G4、A4 |
| midi | INTEGER | MIDI 音高编号 |
| measure_number | INTEGER | 所属小节编号 |
| start_beat | NUMERIC | 音符开始拍位置 |
| duration_beat | NUMERIC | 音符持续拍数 |
| start_seconds | NUMERIC | 标准时间轴开始秒数，可为空 |
| duration_seconds | NUMERIC | 标准时长秒数，可为空 |
| string_name | VARCHAR | 建议琴弦，例如 G、D、A、E，可为空 |
| fingering | VARCHAR | 建议指法，可为空 |
| extra_data | JSONB | 少量扩展数据，可为空 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

#### 约束建议

- score_id 外键关联 scores.id。
- note_index 必须大于等于 1。
- 同一曲谱内 note_index 不应重复。
- 建议建立唯一约束：(score_id, note_index)。
- 建议建立索引：score_id、(score_id, measure_number)。

#### 数据职责

- scores 保存曲谱主信息。
- score_notes 保存音符级标准数据。
- 音准、节奏分析读取 score_notes。
- 普通用户不能直接修改 score_notes。
- 第一版由项目方提前录入少量内置曲谱数据。
- 第一版不实现任意曲谱自动解析后直接写入数据库。

#### 数据库扩展原则

选择独立 score_notes 表的原因：

- 音符是结构化、可排序的数据。
- 后续需要按曲谱、小节、音符查询。
- 更适合音准和节奏分析。
- 避免将大量音符数据塞入单个 JSONB 字段。
- 后续扩展指法、琴弦、时间轴更清晰。
- 更利于数据校验和维护。

该新增表属于项目经理明确确认的必要结构调整，不视为需求漂移。

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
| uploading | 媒体上传中 |
| submitted | 已提交分析 |
| analyzing | 分析中 |
| analyzed | 已完成分析 |
| partially_analyzed | AI 部分模块分析成功，仍可展示有效结果 |
| failed | 分析失败 |
| deleted | 已删除 |

### 8.5 Practice Sessions API 一致性说明

现有字段基本满足 Practice Sessions API。

待后续统一确认字段：

| 字段 | 用途 | 当前处理 |
|---|---|---|
| deleted_at | 记录软删除时间 | 暂不加入 |
| idempotency_key | 数据库级幂等控制 | 暂不加入，可先使用缓存 |

- 本次不增加字段。
- 本次不执行数据库迁移。
- 软删除第一版通过 status = deleted 实现。
- 幂等第一版可通过缓存实现。
- 如后续工程实现需要持久化，再单独提出数据库变更。

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

### 9.5 Practice Media API 一致性说明

现有字段基本满足第一版：

- id
- practice_session_id
- audio_file_url
- video_file_url
- video_orientation
- video_width
- video_height
- duration_seconds
- file_status
- created_at
- updated_at

以下字段列为后续待确认项，不直接加入数据库：

| 字段 | 用途 |
|---|---|
| audio_format | 原始或标准音频格式 |
| video_format | 原始或标准视频格式 |
| audio_size_bytes | 音频文件大小 |
| video_size_bytes | 视频文件大小 |
| video_fps | 视频帧率 |
| audio_sample_rate | 音频采样率 |
| quality_warnings | 质量警告，建议 JSONB |
| error_message | 媒体处理失败原因 |
| checksum | 文件完整性校验 |

- 本次不增加任何字段。
- 本次不执行数据库迁移。
- 后续在数据库统一核对阶段决定最小必要字段。
- 不得一次性加入全部待确认字段。

---

## 10. ai_analysis_tasks AI 分析任务表

### 10.1 表作用

保存 AI 分析任务的执行过程和状态。

### 10.2 建议字段

| 字段 | 类型建议 | 说明 |
|---|---|---|
| id | UUID | 任务 ID |
| practice_session_id | UUID | 关联 practice_sessions.id |
| parent_task_id | UUID | 可为空，关联 ai_analysis_tasks.id，用于表示主任务与子任务关系 |
| task_status | VARCHAR | 任务状态 |
| task_type | VARCHAR | 任务类型 |
| progress | INTEGER | 任务进度，范围为 0—100 |
| current_stage | VARCHAR | 可为空，当前任务阶段 |
| retry_count | INTEGER | 已重试次数，默认 0 |
| max_retries | INTEGER | 最大重试次数，默认值由系统配置确定 |
| started_at | TIMESTAMP | 开始时间 |
| finished_at | TIMESTAMP | 结束时间 |
| error_message | TEXT | 失败原因 |
| created_at | TIMESTAMP | 创建时间 |
| updated_at | TIMESTAMP | 更新时间 |

### 10.3 task_status 建议值

| 值 | 含义 |
|---|---|
| created | 已创建 |
| waiting_for_upload | 等待媒体上传 |
| uploading | 上传中 |
| queued | 已进入任务队列 |
| preprocessing | 媒体预处理中 |
| processing | 分析中 |
| aggregating | 正在汇总结构化结果 |
| generating_feedback | 正在生成自然语言反馈 |
| completed | 已完成 |
| partially_completed | 部分模块完成 |
| failed | 失败 |
| cancelled | 已取消 |

### 10.4 task_type 建议值

| 值 | 含义 |
|---|---|
| full_practice_analysis | 完整练习分析 |
| media_preprocessing | 媒体预处理 |
| pitch_analysis | 音准分析 |
| rhythm_analysis | 节奏分析 |
| posture_analysis | 姿势识别 |
| bowing_analysis | 运弓分析 |
| feedback_generation | 反馈生成 |

### 10.5 主子任务说明

第一版不新增数据库表，可通过 ai_analysis_tasks 现有字段和扩展字段表示主子任务关系。

- `full_practice_analysis` 作为主任务，负责调度和汇总。
- `media_preprocessing`、`pitch_analysis`、`rhythm_analysis`、`posture_analysis`、`bowing_analysis`、`feedback_generation` 作为子任务。
- 子任务的 parent_task_id 关联主任务的 id。
- progress 用于记录任务进度，current_stage 用于记录当前阶段。
- retry_count 和 max_retries 用于实现有限次数的单模块重试。

以上字段仅为数据库设计说明；本次不执行数据库迁移、不生成实际 SQL，也不增加额外表。

### 10.6 Analysis Tasks API 待确认项

后续统一核对以下字段的最小必要集合：

- parent_task_id
- progress
- current_stage
- retry_count
- max_retries
- error_code
- error_message

本节仅记录待确认项，本次不增加字段。

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

### 11.4 Analysis Results API 待确认内容

- 模块状态结构
- 模块评分结构
- overall_rating
- 是否需要 overall_score
- 模型版本信息
- 问题项 JSONB 结构
- warnings 结构

本节仅记录待确认内容，本次不增加字段。

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

### 12.3 Analysis Feedbacks API 待确认内容

- headline
- strengths
- improvements
- next_practice_focus
- encouragement
- llm_model
- prompt_version

本节仅记录待确认内容，本次不增加字段。

以上 Analysis API 数据库扩展项将在后续统一进行最小必要字段核对。本次不执行数据库迁移，不得一次性加入全部待确认字段。

---

## 13. 数据写入流程示例

用户完成一次练习后，数据写入流程如下：

1. 用户选择曲谱。
2. 系统创建 practice_sessions 记录，状态为 recording。
3. 用户完成录音和录像。
4. 系统创建 practice_media 记录。
5. 用户提交 AI 分析。
6. 系统创建 ai_analysis_tasks 主任务，状态为 created。
7. 媒体预处理完成后，创建并调度分析子任务。
8. 子任务完成后汇总结构化结果，写入 ai_analysis_results。
9. AI 生成练习反馈，写入 ai_feedbacks。
10. 根据任务完成情况，practice_sessions.status 更新为 analyzed 或 partially_analyzed。
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
- 9 张核心表
- 结构化字段 + JSONB 扩展字段
- 优先支撑 MVP 练习闭环
- 为未来 AI 分析增强和智能硬件接入预留扩展空间
