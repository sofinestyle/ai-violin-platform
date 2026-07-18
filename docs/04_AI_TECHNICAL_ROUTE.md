# 04_AI_TECHNICAL_ROUTE

## 文档信息

| 项目 | 内容 |
|---|---|
| 文档名称 | AI 技术路线 |
| 项目 | AI Violin Platform |
| 版本 | v0.1 |
| 状态 | DRAFT |
| 负责人 | ChatGPT |
| 创建日期 | 2026-07-11 |

---

## 1. AI 总体目标

AI Violin Platform 的 AI 能力不是为了替代老师，而是为用户提供基础练习反馈。

第一阶段 AI 目标：

- 能听出主要音准问题
- 能判断基础节奏问题
- 能识别明显姿势问题
- 能分析基础运弓问题
- 能生成通俗易懂的练习建议

---

## 2. AI 能力分层

AI 能力分为四层：

```text
数据采集层
↓
信号分析层
↓
结构化评价层
↓
自然语言反馈层
```

### 2.1 数据采集层

采集内容：

- 手机麦克风音频
- 手机摄像头视频
- 用户选择的曲谱
- 练习时间
- 用户学习阶段

### 2.2 信号分析层

分析内容：

- 音高
- 节拍
- 节奏稳定性
- 身体关键点
- 手部动作
- 运弓轨迹

### 2.3 结构化评价层

输出结构化结果，例如：

- 音准评分
- 节奏评分
- 姿势问题列表
- 运弓问题列表
- 小节级错误位置
- 建议练习重点

### 2.4 自然语言反馈层

把结构化结果转化为用户能理解的话。

示例：

> 本次练习整体节奏比较稳定，但第 4 小节有两个音略微偏高。建议先放慢速度练习该小节，再逐步恢复原速。

---

## 3. 音准分析路线

音准分析目标：

- 识别用户演奏音高
- 对比曲谱目标音
- 判断偏高、偏低或准确
- 给出基础反馈

可能技术路线：

- 音频降噪
- 音高检测
- 音符分割
- 与目标曲谱对齐
- 误差计算
- 评分与反馈

第一阶段重点是基础可用，不追求专业级评测。

---

## 4. 节奏分析路线

节奏分析目标：

- 判断用户是否按节拍演奏
- 判断是否抢拍或拖拍
- 判断节奏稳定性

可能技术路线：

- 音频节拍检测
- 起音检测
- 曲谱时间轴对齐
- 节奏偏差计算
- 评分与反馈

---

## 5. 姿势识别路线

姿势识别目标：

- 判断用户基础演奏姿势是否明显错误

可能技术路线：

- 手机摄像头采集视频
- 人体关键点识别
- 手部关键点识别
- 持琴姿势规则判断
- 右手持弓规则判断
- 反馈生成

第一阶段可先做基础提醒，例如：

- 肩部过高
- 头部过度倾斜
- 左手手腕塌陷
- 右手持弓位置异常

---

## 6. 运弓分析路线

运弓分析目标：

- 判断弓运行是否基本稳定

可能技术路线：

- 视频识别琴弓位置
- 跟踪弓运动轨迹
- 判断弓与琴桥关系
- 判断弓速变化
- 判断运弓方向和稳定性

运弓分析技术难度较高，但已纳入第一阶段 MVP；第一阶段仅提供基础反馈，后续再提高分析精度。

---

## 7. AI 陪练反馈路线

AI 陪练的输入：

- 用户练习记录
- 音准分析结果
- 节奏分析结果
- 姿势识别结果
- 运弓分析结果
- 用户学习阶段
- 当前曲谱难度

AI 陪练输出：

- 本次练习总结
- 主要问题
- 优先改进点
- 下一步练习建议
- 鼓励性反馈

---

## 8. 第一阶段技术优先级

建议优先级：

| 优先级 | AI 能力 | 原因 |
|---|---|---|
| P0 | 音频录制与音准分析 | 小提琴学习最核心问题之一 |
| P0 | 节奏分析 | 与练习效果直接相关 |
| P0 | AI 反馈总结 | 用户最容易感知价值 |
| P0 | 姿势识别 | MVP 基础姿势反馈能力 |
| P0 | 运弓分析 | MVP 基础运弓反馈能力 |
| P1 | 更高精度的姿势识别 | 后续提升识别准确性和适配性 |
| P1 | 更高精度的运弓分析 | 后续提升轨迹和稳定性分析准确性 |
| P1 | 曲谱智能解析增强 | 后续提升曲谱理解与对齐能力 |
| P2 | 智能指板 | 后续硬件能力 |
| P2 | 智能琴弓 | 后续硬件能力 |
| P2 | 智能小提琴 | 后续硬件能力 |
| P2 | 硬件传感器数据接入 | 后续硬件能力 |

---

## 9. 技术风险

主要风险：

- 手机麦克风质量不稳定
- 环境噪音影响音频分析
- 初学者音符不清晰，音高检测困难
- 曲谱与演奏对齐有难度
- 姿势识别受拍摄角度影响
- 运弓识别技术难度高
- AI 反馈需要避免误导用户

---

## 10. 风险控制思路

第一阶段应降低复杂度：

- 先支持简单曲谱
- 先支持单音旋律
- 先支持慢速练习
- 先支持安静环境
- 先给出基础反馈
- 不做过度专业评分
- 对 AI 不确定结果明确提示

示例：

> 本次分析结果可能受环境噪音影响，仅供练习参考。

---

## 11. 后续硬件协同方向

未来接入智能硬件后，可以增强：

- 左手按弦位置检测
- 右手运弓压力检测
- 琴弓速度检测
- 触弦位置检测
- 实时灯光或震动反馈
- 更准确的练习数据采集

硬件不是第一阶段前提，而是后续提升学习体验的方向。

---

## 12. AI Pipeline

第一阶段采用 Pipeline 架构。

第一版内置曲谱采用人工准备或工具辅助生成的结构化基准数据。曲谱主数据存储在 scores，音符级基准数据存储在 score_notes；音准分析和节奏分析以 score_notes 为标准答案。第一版不实时解析任意曲谱图片。后续曲谱解析能力可以生成 score_notes，但不改变下游分析协议。

媒体预处理标准输入：

- 音频：WAV、单声道、44.1 kHz
- 视频：MP4、H.264

- 原始上传格式可以是 m4a、aac、wav、mp4、mov。
- Media Service 负责格式转换和基础质量检查。
- AI Pipeline 只读取已经完成预处理或已确认可用的媒体。
- AI Pipeline 不直接信任客户端提供的媒体元数据。
- 最终时长、分辨率、帧率和采样率以后端实际解析结果为准。

传统 AI 负责：

- 检测
- 识别
- 评分

LLM 只负责：

- 总结
- 建议

避免 LLM 直接产生不可解释分析结果。

专业模型和算法生成结构化结果，由 Aggregation Service 汇总，再由 LLM 生成反馈：

```text
专业模型和算法
↓
生成结构化结果
↓
Aggregation Service 汇总
↓
LLM 生成反馈
```

- 音准、节奏、姿势、运弓由专业模型和规则引擎负责。
- LLM 不承担核心检测和评分。
- LLM 输入必须是标准化结构结果。
- LLM 输出必须受结构化字段和 Prompt 规则约束。
- 低置信度结果不得转化为确定性反馈。
- Feedback 失败时不得丢失已完成的结构化结果。
- 模型版本和 Prompt 版本后续需支持追踪，但本阶段不修改数据库。

---

## 13. AI 服务与 API 的关系

FastAPI 负责：

- 用户请求
- 权限
- 数据管理
- Task 调度

Python AI Service 负责：

- Media Preprocessing
- Pitch
- Rhythm
- Posture
- Bowing
- Aggregation

LLM 负责：

- Feedback Generation

LLM 不直接分析音视频。

---

## Phase 7.1：AI 模块公共规范（AI Module Contract）

本章节定义 AI Pipeline 内部模块的公共运行契约。该契约仅约束 Python AI Service 内部模块之间的输入、输出、错误、重试与运行元数据，不修改已 Freeze 的 API、数据库结构、任务状态机或 Pipeline 流程。

### 1. 模块职责边界

AI Pipeline 中的模块职责如下：

| 模块 | 职责 |
|---|---|
| `pitch_analysis` | 基于已预处理音频完成音高识别、曲谱基准对齐、音准问题识别与评分。 |
| `rhythm_analysis` | 基于已预处理音频和曲谱时间轴完成起音、节奏对齐、稳定性问题识别与评分。 |
| `posture_analysis` | 基于已预处理视频完成身体与手部关键点相关的基础姿势识别、问题识别与评分。 |
| `bowing_analysis` | 基于已预处理视频完成琴弓位置、轨迹和基础稳定性相关的分析、问题识别与评分。 |
| `aggregation` | 汇总各模块已保存的标准化结果，生成模块汇总、综合评价及可供反馈使用的结构化输入。 |
| `feedback_generation` | 基于已保存的结构化结果、用户学习阶段和曲谱信息生成面向用户的总结、教学解释与练习建议。 |

每个模块只负责自身领域，不得替代其他模块的检测、推理或评分职责。

LLM 不负责：

- 音视频识别
- 推理
- 检测
- 打分

LLM 仅负责：

- 总结
- 教学解释
- 练习建议

### 2. 统一模块输入

每个 AI 模块使用统一输入上下文。上下文至少包括：

- `taskId`
- `practiceSessionId`
- `scoreReference`
- `media`
- `userContext`
- `execution`

各字段仅用于模块职责范围内的处理：

- `scoreReference` 提供由后端准备的曲谱基准数据。
- `media` 提供 Media Preprocessing 已输出并确认可用的标准化媒体及其质量信息。
- `userContext` 提供用户学习阶段等允许用于反馈和评价的上下文。
- `execution` 提供当前模块、执行尝试和受控运行配置。

模块只能读取 Media Preprocessing 的输出，不能直接读取客户端原始媒体，也不得信任客户端提供的媒体元数据。

### 3. 统一模块输出

模块面向 Aggregation Service 输出的标准化结果必须与 Phase 6 已 Freeze 的 Analysis Result 协议保持一致，字段名称不得修改。统一输出字段为：

- `status`
- `score`
- `rating`
- `confidence`
- `issues`
- `warnings`
- `rawResult`

其中 `status`、`score`、`rating`、`confidence`、`issues` 和 `warnings` 用于既有结构化结果；`rawResult` 仅用于内部保存和排障，不直接返回 APP。

### 4. 状态规范

模块内部状态统一为：

- `completed`
- `failed`
- `insufficient_data`
- `cancelled`

`insufficient_data` 表示当前模块的数据不足以形成可靠分析结论，例如有效画面、声音或曲谱对齐信息不足。它不是 `failed`；模块已完成可执行的判断，但不能给出足够可靠的结果。

该内部状态规范不修改既有主任务状态机和对外 API 状态。

### 5. Score 与 Confidence

`score` 表示用户在该模块评估维度上的表现；`confidence` 表示模型或规则对该分析结果的可信度。

- 两者不得混用。
- 不得因 `confidence` 自动修改 `score`。
- 低 `confidence` 应通过 `warnings`、不确定表达或 `rating = insufficient_data` 处理，并继续遵守既有低置信度展示规则。

### 6. Issue 与 Warning

`issue` 表示用户练习中识别到的问题，例如音高偏差、节奏不稳、肩部过高或弓线倾斜。

`warning` 表示分析可靠性提醒，不应被表述为用户练习错误。例如：

- 噪音过高
- 光线不足
- 拍摄角度异常
- 视频不完整

模块必须将用户表现问题和分析可靠性提醒分别写入 `issues` 与 `warnings`。

### 7. Raw Result

`rawResult` 用于保存仅供内部处理、复核和排障使用的原始或中间结果，可包括：

- 人体关键点
- 手部关键点
- 琴弓轨迹
- 模型中间输出
- 调试信息

APP 不直接读取 `rawResult`。对普通用户的返回继续使用 Phase 6 已定义的标准化结构结果。

### 8. 模块错误分类

AI Service 内部统一使用以下错误分类：

- `input_error`
- `media_error`
- `quality_error`
- `model_error`
- `timeout_error`
- `persistence_error`
- `internal_error`

该分类仅作为 AI Service 内部规范，用于记录、排障和重试判断；不得替代或修改既有对外 API 错误码。

### 9. 模块幂等

模块执行实例的唯一标识为：

`taskId + module + attempt`

同一执行实例的重复调度不得产生冲突结果。成功结果不得被失败结果覆盖；重试仅能写入对应尝试的结果，并由既有任务流程依据有效结果重新汇总。

### 10. Runtime Metadata

每个模块结果可在内部 `runtimeMetadata` 中记录以下运行元数据：

- `modelName`
- `modelVersion`
- `ruleVersion`
- `pipelineVersion`
- `processingTimeMs`

第一版仅将 `runtimeMetadata` 作为 JSONB 内部元数据使用，不新增数据库字段，也不改变对外 API 返回字段。

---

## Phase 7.2：媒体预处理模块详细设计

本章节定义 `media_preprocessing` 模块的详细设计。它遵循 Phase 5、Phase 6 与 Phase 7.1 已 Freeze 的架构和模块契约；本章节中的 `qualityReport`、`moduleEligibility`、`runtimeMetadata` 及内部 URI 均为 AI Service 内部输出、JSONB 内部数据或受控存储引用，不新增数据库列或外部 API 字段。

### 1. 模块定位

`media_preprocessing` 是所有 AI 分析模块的统一媒体入口，负责：

- 解析客户端上传的音频和视频。
- 验证文件是否真实可用，并读取真实媒体元数据。
- 转换为统一格式，进行基础质量检测和音视频时间轴校验。
- 生成标准化媒体产物、`Media Quality Report` 与 `Module Eligibility`。
- 为下游 AI 模块提供统一输入。

该模块不负责音高检测、节奏评分、姿势判断、琴弓轨迹评价、综合评分、LLM Feedback 或用户演奏技术判断。

例如：“画面过暗”属于媒体预处理；“右肩过高”属于姿势识别；“琴弓不可见”属于媒体可分析性问题；“弓线倾斜”属于运弓分析问题。

### 2. 总体流程

```text
客户端原始媒体
↓
文件完整性检查
↓
真实格式和元数据解析
↓
音频 / 视频标准化
↓
基础质量检测
↓
音视频时间轴校验
↓
生成标准化媒体产物
↓
生成 Media Quality Report
↓
生成 Module Eligibility
↓
进入 Pitch / Rhythm / Posture / Bowing
```

### 3. Media Service 与 Worker 职责边界

Media Service（FastAPI）负责：

- 用户权限检查、接收媒体上传和文件大小、上传协议检查。
- 创建和更新 `practice_media`、保存原始媒体。
- 投递 `media_preprocessing` 子任务、返回媒体处理状态。
- 管理存储引用和访问权限。

Media Preprocessing Worker（Python AI Service）负责：

- 读取内部受控存储中的原始媒体。
- 解析真实媒体参数，完成音频提取与转码、视频方向纠正与转码。
- 执行基础降噪、抽帧、质量检测和时间轴检查。
- 生成标准化媒体、质量报告和模块可执行性。

### 4. 统一输入契约

内部输入至少包含：

- `taskId`
- `practiceSessionId`
- `module`
- `media.audio`
- `media.video`
- `execution`

`media.audio` 和 `media.video` 可包含 `mediaId`、`sourceUri`、`declaredMimeType`、`declaredFileSize` 与 `declaredOrientation`。

- `sourceUri` 必须是内部受控存储引用，Worker 不接受公开 URL 作为正式生产输入。
- 不信任客户端声明的 MIME Type、后缀、时长、分辨率、帧率、采样率、声道数或视频方向；所有真实参数必须由后端媒体解析工具获取。
- 音频或视频允许单独缺失；两者均不可用时，不得进入正常 AI 分析。

### 5. 媒体工具路线

第一版建议使用 FFmpeg 完成音频提取与转码、视频转码、采样率与声道统一、视频方向纠正、帧率处理、抽帧和时间轴处理；使用 FFprobe 解析 `codec`、`container`、`duration`、`sampleRate`、`channels`、`width`、`height`、`frameRate`、`rotation`、`stream`、`startTime` 和 `timeBase`。

Python 辅助库可包括 `numpy`、`scipy`、`librosa`、`soundfile` 与 `opencv-python`，用于质量特征计算。FFmpeg / FFprobe 负责媒体工程处理，Python 库负责质量特征计算；本阶段只确定技术方向，不写死具体依赖版本。

### 6. 音频预处理

输入支持已 Freeze 的 `m4a`、`aac`、`wav` 以及视频中的音轨，真实支持情况以解码结果为准，而不是文件后缀。标准输出为 WAV、PCM、Mono、44.1 kHz，建议 codec 为 `pcm_s16le`。

```text
读取原始媒体
↓
识别真实音轨
↓
检查是否可解码
↓
提取音频
↓
统一为 Mono
↓
统一为 44.1 kHz
↓
基础音量检查
↓
保守降噪
↓
静音和削波检查
↓
输出标准 WAV
```

### 7. 降噪原则

第一版只做保守降噪，不得音高校正、节奏修正、删除错误音、过度去除小提琴泛音，也不得使用明显改变起音时间的激进处理。

建议同时保留标准化未降噪音频和轻度降噪分析音频。下游默认读取轻度降噪版本，必要时可回退至标准化原音。

### 8. 音频质量检测

至少检测 RMS 音量、Peak 音量、动态范围、静音比例、削波比例、环境噪声、时长、解码稳定性和时间轴连续性。Warning Code 包括：

- `audio_volume_too_low`
- `audio_clipping_detected`
- `background_noise_high`
- `audio_silence_excessive`
- `audio_duration_invalid`
- `audio_decode_unstable`
- `audio_track_missing`
- `audio_timeline_discontinuous`

具体阈值由工程配置和测试确定，本 SPEC 不写死。

### 9. 视频预处理

输入支持 `mp4`、`mov`；标准输出为 MP4、H.264。预处理必须保留原始宽高比例，不拉伸、不随意裁剪、不改变人物和乐器几何比例；应纠正视频方向并保持音视频时间轴。过高分辨率可合理缩放，但不得放大小分辨率视频。

### 10. 视频方向处理

实际方向判断优先级为：

1. FFprobe Rotation Metadata
2. 实际 `width` / `height`
3. 客户端方向信息（仅作辅助）

纠正后必须保证演奏者方向正确、后续关键点模型可正常读取，并且不破坏音视频同步关系。

### 11. 分辨率、帧率和抽帧

本阶段不写死统一目标分辨率或 FPS，具体数值由后续工程测试确定。姿势分析可使用较低采样率；运弓分析需要更连续的帧序列；不得将所有视频统一降到过低帧率。

预处理输出包括标准化视频、质量检测采样帧和带时间戳的 Frame Index。Frame Index 至少包括 `frameIndex`、`timestampSecond`、`sourceFrameNumber`，不得只输出没有时间戳的图片序列。

### 12. 视频质量检测

基础检测包括平均亮度、过暗比例、过曝比例、模糊比例、黑屏比例、冻结帧或重复帧、严重画面抖动、人体是否存在、身体关键区域是否基本可见、小提琴是否基本可见，以及琴弓是否在足够帧中可见。

此处只判断是否具备分析条件，不得输出持琴姿势是否正确、弓线是否正确、运弓是否稳定或肩部姿势问题。

Video Warning Code 包括：

- `video_too_dark`
- `video_overexposed`
- `video_blurry`
- `video_unstable`
- `video_orientation_corrected`
- `person_not_detected`
- `person_partially_visible`
- `violin_not_visible`
- `bow_not_visible`
- `video_frames_incomplete`
- `video_duration_invalid`
- `video_decode_unstable`

`bow_not_visible` 只表示琴弓在视频中不可见或可见帧不足，属于媒体可分析性提醒；不得与运弓模块对弓线和轨迹的技术评价混用。

### 13. 音视频时间轴

统一使用相对于练习开始的 `0.000` 秒。分别记录原始起始时间、原始时长、标准化后时长、时间偏移、是否裁剪、是否补齐及时间轴是否连续。

至少检查音频与视频时长差、起始时间差、音轨是否晚于视频、异常空白头、时间跳变及标准化后时间轴连续性。第一版只做基础检查和轻量纠正。

严重不同步时，音频模块和视频模块仍可独立执行，跨模块时间关联必须产生 Warning，不得伪造精确同步关系。

### 14. Media Quality Report

统一输出 `qualityReport`：

```text
audio: status, durationSeconds, sampleRate, channels, rmsDb, peakDb,
       silenceRatio, clippingRatio, warnings
video: status, durationSeconds, width, height, frameRate, orientation,
       darkFrameRatio, blurFrameRatio, personVisibility, violinVisibility,
       bowVisibility, warnings
synchronization: status, durationDifferenceSeconds, warnings
```

媒体内部质量状态统一为 `usable`、`usable_with_warnings`、`insufficient_data`、`unusable`。这些属于 AI Service 内部质量状态，不修改外部 API 状态机。

### 15. Module Eligibility

预处理必须明确返回 `pitch`、`rhythm`、`posture`、`bowing` 的 `eligible` 与 `reasonCodes`。

- Pitch：音频可解码、存在有效声音、时长基本有效、音量非严重过低、噪声未达到完全不可用且曲谱基准数据存在。
- Rhythm：音频可解码、可检测足够起音信息、时间轴连续、曲谱节奏基准存在且静音比例没有严重异常。
- Posture：视频可解码、方向正确、演奏者存在、关键身体区域基本可见且亮度、清晰度基本可用。
- Bowing：视频可解码、演奏者和小提琴可见、琴弓在足够连续帧中可见、帧率和清晰度基本满足轨迹分析且画面没有长期遮挡。

若音频可用、视频不可用，Pitch 与 Rhythm 执行，Posture 与 Bowing 跳过；主任务可进入既有 `partially_completed`，不应直接 `failed`。

### 16. 统一输出契约

`media_preprocessing` 输出保持 Phase 7.1 Module Contract，包括 `module`、`status`、`score`、`rating`、`confidence`、`issues`、`warnings`、`rawResult`、`outputs`、`qualityReport`、`moduleEligibility` 和 `runtimeMetadata`。

该模块不对用户演奏评分，因此 `score = null`、`rating = null`。`outputs` 的内部引用可包括：

- audio：`normalizedUri`、`denoisedUri`、`format`、`codec`、`sampleRate`、`channels`、`durationSeconds`
- video：`normalizedUri`、`format`、`codec`、`width`、`height`、`frameRate`、`durationSeconds`、`orientation`
- `frameIndexUri`

内部 URI 不得返回 APP。

### 17. 产物和存储

产物可包括原始上传文件、标准化音频、轻度降噪音频、标准化视频、抽帧索引、质量报告和调试元数据。

- PostgreSQL 只保存结构化元数据和内部引用；大型媒体存放于对象存储或受控文件存储。
- 不将完整音视频二进制写入 PostgreSQL，APP 不获得内部真实路径。
- 媒体访问必须进行权限控制，下游 Worker 使用内部服务权限读取。
- 临时文件必须按任务隔离、处理后清理、不与其他用户共享文件名，且不得直接使用用户原始文件名作为服务器路径。

### 18. 错误和重试

通常可重试的情况包括 FFmpeg 临时失败、Worker 异常退出、对象存储读取超时、临时磁盘或网络错误、服务器资源不足和结果保存失败。

文件真实损坏、没有音频流、没有视频流、视频全黑、时长为零、媒体完全无法解码或画面完全不具备分析条件，通常不应重复重试；应返回明确质量状态，不得无限重试。

已成功预处理产物不得被失败重试覆盖，失败重试不得删除成功产物，下游读取当前有效版本。

### 19. 隐私和安全

- 原始媒体默认私有，禁止使用公开访问链接。
- 普通日志不得写入媒体完整 URL，不得向 APP 返回服务器绝对路径。
- 调试日志不得包含完整用户身份；Worker 只能访问当前任务所需媒体。
- 临时文件及时清理，用户媒体不得默认用于模型训练；后续训练用途必须另行获得明确授权。

### 20. Phase 7.2 Freeze

本阶段 Freeze：

- `media_preprocessing` 职责边界及 Media Service 与 AI Worker 职责分离。
- 不信任客户端媒体元数据。
- 音频统一为 WAV / PCM / Mono / 44.1 kHz，视频统一为 MP4 / H.264，并纠正视频方向。
- 不拉伸、不随意裁剪，采用保守降噪。
- Media Quality Report、Module Eligibility、音频与视频部分可用，以及四模块根据 Eligibility 独立执行。
- 严重不同步时不伪造同步关系。
- 媒体文件不存入 PostgreSQL，原始媒体默认私有，用户媒体不默认用于训练。
- 不修改数据库或 API；具体阈值、分辨率和 FPS 由后续工程测试配置。

---

## Phase 7.3：Pitch Analysis（音准分析模块）

本章节定义 Pitch Analysis 的详细设计，遵循已 Freeze 的 AI Module Contract 与 Media Preprocessing 输出，不修改数据库、API、主任务和子任务架构或其他模块职责。

### 1. 模块定位

Pitch Analysis 负责音高检测、音符候选识别、音符分割、与 `score_notes` 对齐、cents 偏差计算、偏高/偏低判断、错音/漏音/多余音识别、音准评分和模块置信度。

该模块不负责节奏评分、姿势分析、运弓分析、LLM Feedback、自动修音或调音器功能。

### 2. Pitch Pipeline

```text
标准化音频
↓
音频有效区间检测
↓
Pitch Detection
↓
Voiced / Unvoiced
↓
音符候选分割
↓
滑音、抖动处理
↓
Score Alignment
↓
Note Evaluation
↓
Measure Summary
↓
Pitch Result
```

### 3. 输入契约

统一输入至少包括：

- `taskId`
- `practiceSessionId`
- `normalizedAudio`
- `denoisedAudio`
- `scoreReference`
- `qualityContext`
- `userContext`
- `execution`

模块必须读取 `score_notes` 作为曲谱基准，不得相信客户端提交的目标音符。

### 4. 曲谱标准数据

Pitch Analysis 使用 `score_notes`，至少读取：

- `MIDI`
- `expectedPitch`
- `expectedStartSecond`
- `expectedDurationSecond`
- `measure`
- `beat`

第一版不实时解析任意曲谱图片。

### 5. Pitch Detection

Pitch Detection 输出连续音高轨迹，每个采样点至少包括 `timestamp`、`frequency`、`midiFloat`、`confidence`、`voiced`。

本阶段不锁定 YIN、pYIN、CREPE 或其他具体算法；模型可以替换，但输出协议必须保持一致。

### 6. Voiced / Unvoiced

模块必须区分乐音、静音、噪声和不确定区间，避免噪声生成虚假音符。

### 7. Pitch Smoothing

允许使用 Median、平滑、异常点过滤和连续区间合并，以提高轨迹稳定性。不得修正用户音高，不得人为提高分数。

### 8. Note Segmentation

音符候选可来自起音、Pitch Jump、静音、曲谱时间和音量变化。输出为 Detected Note，供后续与 `score_notes` 建立对应关系。

### 9. Score Alignment

Pitch 与 `score_notes` 进行序列对齐，可采用 DTW、Sequence Alignment、DP 或其他可替换算法。本模块只负责音符对应关系，不负责节奏评分。

### 10. Cents

统一使用 `deviationCents`：正值表示偏高，负值表示偏低。具体判断阈值由后续工程配置和测试确定，本阶段不写死。

### 11. 音符状态

统一音符状态：

- `accurate`
- `slightly_high`
- `high`
- `slightly_low`
- `low`
- `wrong_note`
- `missed`
- `extra`
- `insufficient_data`

### 12. Vibrato / Slide

第一版兼容 Vibrato 和 Slide 对音高轨迹的影响，但不进行专业评价。必要时可将滑音过程与稳定音区分，避免将其直接误判为持续音准错误。

### 13. Harmonic

模块兼容空弦、泛音和倍频误判，可使用 Harmonic Consistency 辅助判断，避免将倍频或泛音现象直接识别为错误音。

### 14. Polyphonic

第一版正式支持单音旋律，不支持双音、和弦或多声部评分。检测到多声部音频时应输出相应 Warning，不得伪造单音评分结果。

### 15. Issue Code

统一 Issue Code：

- `pitch_high`
- `pitch_low`
- `wrong_note`
- `missed_note`
- `extra_note`
- `pitch_unstable`
- `pitch_slide_into_note`
- `pitch_insufficient_data`

### 16. Warning

统一 Warning：

- `background_noise_high`
- `pitch_tracking_unstable`
- `harmonic_ambiguity`
- `polyphonic_audio_detected`
- `score_alignment_low_confidence`
- `unsupported_pitch_range`

### 17. Pitch Score

Pitch Score 综合准确率、偏差比例、错音、漏音、多余音和稳定性，输出 `score`、`rating`、`confidence`。`score` 表示用户表现，`confidence` 表示分析可信度，不得因 `confidence` 修改 `score`。

### 18. Runtime Metadata

统一使用 `runtimeMetadata`，包括 `modelName`、`modelVersion`、`ruleVersion`、`pipelineVersion`、`processingTimeMs`。这些属于 JSONB 内部数据，不新增数据库字段或外部 API 字段。

### 19. MVP 范围

第一版范围：

- 单音旋律
- 初学者曲谱
- 不支持双音评分
- 不支持专业 Vibrato 评价
- 不支持实时调音

### 20. Phase 7.3 Freeze

本阶段 Freeze：

- Pitch Boundary
- Pitch Pipeline
- Pitch Detection
- Score Alignment
- Cents
- Note Status
- Pitch Issue Code
- Pitch Warning
- Pitch Score
- Runtime Metadata
- MVP Scope

---

## Phase 7.4：Rhythm Analysis（节奏分析模块）

本章节定义 Rhythm Analysis 的详细设计，遵循已 Freeze 的 AI Module Contract、Media Preprocessing 与 Pitch Analysis 边界，不修改数据库、API、业务代码、主 Pipeline 或既有模块。

### 1. 模块定位

Rhythm Analysis 负责 Beat Detection、Onset Detection、Tempo Estimation、Rhythm Segmentation、Rhythm Alignment、Timing Evaluation、Rhythm Stability 和 Rhythm Score。

该模块不负责 Pitch、Posture、Bowing 或 AI Feedback。

### 2. Rhythm Pipeline

```text
Normalized Audio
↓
Onset Detection
↓
Beat Tracking
↓
Tempo Estimation
↓
Rhythm Segmentation
↓
Score Alignment
↓
Timing Evaluation
↓
Measure Summary
↓
Rhythm Result
```

### 3. 输入契约

输入至少包含：

- `normalizedAudio`
- `score_notes`
- `score_measures`
- `tempoReference`
- `qualityContext`
- `executionContext`

模块不得直接读取客户端原始媒体，必须使用 Media Preprocessing 的标准化音频和质量上下文。

### 4. Rhythm Reference

统一从曲谱基准读取 BPM、Time Signature、Measure、Beat、Expected Start 与 Expected Duration。第一版支持 `4/4`、`3/4`、`2/4`、`6/8`。

### 5. Onset Detection

Onset Detection 用于检测音符起始点。允许使用不同算法，但必须保持统一输出协议，供后续分段、对齐和时间评价使用。

### 6. Beat Tracking

统一处理 Beat、Downbeat 和 Tempo Drift。具体算法不在本阶段锁定。

### 7. Tempo Estimation

统一输出 Estimated BPM、Tempo Stability 和 Tempo Drift，并明确区分用户 Tempo 与 Score Tempo。

### 8. Rhythm Alignment

Rhythm 与 `score_notes` 进行序列对齐，可采用 DTW、DP、Sequence Alignment 或其他可替换算法。

### 9. Timing Evaluation

每个音符统一输出 `on_time`、`early`、`late`、`missed`、`extra` 之一，并输出 `timingOffsetMs`。

### 10. Rhythm Stability

分析连续提前、连续滞后、Tempo 波动与节奏稳定性。

### 11. Issue Code

统一 Issue Code：

- `rhythm_early`
- `rhythm_late`
- `rhythm_unstable`
- `missed_beat`
- `extra_beat`
- `tempo_drift`

### 12. Warning Code

统一 Warning Code：

- `beat_tracking_low_confidence`
- `noisy_audio`
- `insufficient_onset`
- `tempo_unstable`
- `unsupported_time_signature`

### 13. Rhythm Score

统一输出 `score`、`rating`、`confidence`。`confidence` 表示分析可信度，不得修改 `score`。

### 14. Runtime Metadata

`runtimeMetadata` 至少包括 `modelVersion`、`pipelineVersion`、`processingTimeMs`、`onsetAlgorithm`、`tempoAlgorithm`，属于 JSONB 内部数据，不新增数据库字段或外部 API 字段。

### 15. Rhythm Fusion Context

预留 Rhythm Fusion Context 扩展点。第一版仅使用音频；未来可融合视频起弓动作、弓向切换等视觉信息。本阶段仅定义扩展点，不新增数据库、不修改 API。

### 16. MVP Scope

第一版支持单旋律、基础节奏分析、Tempo Estimation、Beat Tracking 和 Timing Evaluation。

第一版不支持 Rubato、多人同步、合奏分析或专业节奏风格评价。

### 17. Phase 7.4 Freeze

本阶段 Freeze：

- Rhythm Boundary
- Rhythm Pipeline
- Rhythm Reference
- Beat Tracking
- Tempo Estimation
- Rhythm Alignment
- Timing Evaluation
- Rhythm Stability
- Rhythm Issue Code
- Rhythm Warning
- Rhythm Score
- Runtime Metadata
- Rhythm Fusion Context
- MVP Scope
