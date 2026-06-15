# Agnes Video Generator v2.0 — 大版本回归测试报告

| 元数据 | 值 |
|--------|-----|
| 日期 | 2026-06-15 00:56 UTC |
| 版本 | b584dec feat: generate single narration for entire video instead of per-scene |
| 报告版本 | 2.0 |
| 自动验证 | 80/147 通过 |

## 概览

| 状态 | 数量 |
|------|------|
| 总计 | 9 |
| ✅ 完成 | 7 |
| ❌ 失败 | 2 |
| ⏭️ 跳过 | 0 |
| 🔄 运行中 | 0 |
| ⏳ 待处理 | 0 |

端点验证: 9/9 ✅

---

## 简单视频 (Simple)

### S1 纯文本 t2v — ⚠️ 通过但有失败检查 (160.9s)
### S2 图生视频 ti2vid — ⚠️ 通过但有失败检查 (160.9s)
### S3 关键帧 keyframes — ⚠️ 通过但有失败检查 (207.4s)

| 检查项 | S1 | S2 | S3 |
|---|---|---|---|
| F1_final_video_exists | ✅ | ✅ | ✅ |
| F1_final_video_nonempty | ✅ | ✅ | ✅ |
| F2_duration_gt_0 | ✅ | ✅ | ✅ |
| F4_has_audio_stream | ✅ | ✅ | ✅ |
| F4_has_speech | ❌ | ❌ | ❌ |
| F6_text_match | ❌ | ❌ | ❌ |
| F7_duration_reasonable | ✅ | ✅ | ✅ |
| R10_full_subtitle | N/A | N/A | N/A |
| R1_task_state_valid | ✅ | ✅ | ✅ |
| R2_task_type | simple | simple | simple |
| R2_task_type_matches | ✅ | ✅ | ✅ |
| R3_all_completed | N/A | N/A | N/A |
| R4_final_path_exists | ✅ | ✅ | ✅ |
| R5_has_video_id | ✅ | ✅ | ✅ |
| R5_task_json | ✅ | ✅ | ✅ |
| R6_curl_sh | ✅ | ✅ | ✅ |
| R6_has_video_id_in_curl | ✅ | ✅ | ✅ |
| R7_audio_files | N/A | N/A | N/A |
| R7_sub_dirs_exist | N/A | N/A | N/A |
| R8_subtitle_srt | N/A | N/A | N/A |
| R9_full_narration | N/A | N/A | N/A |

---

## 创意视频 (Creative)

### C1 纯文字+独立+无配音 — ⚠️ 通过但有失败检查 (207.4s)
### C2 带参考图+关键帧+无配音 — ⚠️ 通过但有失败检查 (535.8s)
### C3 参考图生成尾帧+关键帧+无配音 — ⚠️ 通过但有失败检查 (414.1s)
### C4 独立场景+配音字幕验证 — ⚠️ 通过但有失败检查 (469.2s)

| 检查项 | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| F1_final_video_exists | ✅ | ✅ | ✅ | ✅ |
| F1_final_video_nonempty | ✅ | ✅ | ✅ | ✅ |
| F2_duration_gt_0 | ✅ | ✅ | ✅ | ✅ |
| F4_has_audio_stream | ✅ | ✅ | ✅ | ✅ |
| F4_has_speech | N/A | N/A | N/A | ✅ |
| F6_text_match | N/A | N/A | N/A | ✅ |
| F7_duration_reasonable | ✅ | ✅ | ✅ | ✅ |
| R10_full_subtitle | N/A | N/A | N/A | N/A |
| R1_task_state_valid | ✅ | ✅ | ✅ | ✅ |
| R2_task_type | creative | creative | creative | creative |
| R2_task_type_matches | ✅ | ✅ | ✅ | ✅ |
| R3_all_completed | ❌ | ✅ | ✅ | ❌ |
| R4_final_path_exists | ✅ | ✅ | ✅ | ✅ |
| R5_has_video_id | ❌ | ❌ | ❌ | ❌ |
| R5_task_json | ❌ | ❌ | ❌ | ❌ |
| R6_curl_sh | ❌ | ❌ | ❌ | ❌ |
| R6_has_video_id_in_curl | ❌ | ❌ | ❌ | ❌ |
| R7_audio_files | ❌ | ❌ | ❌ | ❌ |
| R7_sub_dirs_exist | ✅ | ✅ | ✅ | ✅ |
| R8_subtitle_srt | ✅ | ✅ | ✅ | ✅ |
| R9_full_narration | N/A | N/A | N/A | N/A |

---

## 稿件视频 (Manuscript)

### M1 短稿件+配音 — ❌ failed
### M2 短稿件+自定义字幕 — ❌ failed

| 检查项 | M1 | M2 |
|---|---|---|

---

## 端点验证 (E1-E9)

| 端点 | 状态 | 详情 |
|------|------|------|
| E1 | ✅ | 200 |
| E2 | ✅ | 200 |
| E3 | ✅ | 200 |
| E4 | ✅ | 200 |
| E5 | ✅ | 200 |
| E6 | ✅ | 200 |
| E7 | ✅ | eedfa570ed95 type=creative |
| E8 | ✅ | d5b66dc2d611 200 |
| E9 | ✅ | no suitable task for stop (skip) |

---

## 需手动验证

以下检查因 IMAX 视觉限制无法由脚本验证，需人工确认：

| 检查项 | 操作 | 预期 |
|--------|------|------|
| F5 字幕可见性 | 播放 final_video.mp4 观察画面 | 字幕内容、位置、样式与配置一致 |

> 音频正确性 (F4) 和字幕文本匹配 (F6) 已由脚本通过 whisper ASR 自动验证。

## 错误汇总

- **C1** (纯文字+独立+无配音): R3_all_completed
- **C2** (带参考图+关键帧+无配音): R5_task_json
- **C3** (参考图生成尾帧+关键帧+无配音): R5_task_json
- **C4** (独立场景+配音字幕验证): R3_all_completed
- **M1** (短稿件+配音): status=failed: ?
- **M2** (短稿件+自定义字幕): status=failed: ?
- **S1** (纯文本 t2v): F4_has_speech
- **S2** (图生视频 ti2vid): F4_has_speech
- **S3** (关键帧 keyframes): F4_has_speech
