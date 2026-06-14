# Agnes Video Generator v2.0 — 测试计划

> **当前阶段**：🟢 测试计划就绪 — 等待各批次代码实现完成后逐批启动 QA
> **执行模式**：分批验证（Batch A → Batch B → Batch C），每批独立测试
> **配套文档**：`AGENTS.md`（测试规范）、`docs/development_plan.md`（总计划 + 验证清单）

*版本：v2.0 | 日期：2025-06-14*

---

## 一、测试范围

| 维度 | 覆盖 |
|------|------|
| Batch A | 5 个文件（models/task.py, models/__init__.py, core/config.py, core/task_manager.py, requirements.txt） |
| Batch B | 14 个文件（api/×3, audio/×2, compositor/×2, pipelines/×3, screenwriter.py, 各 __init__.py） |
| Batch C | 3 个文件（server.py, core/__init__.py, static/index.html） |

---

## 二、分批验证清单

### Batch A 验证（基础设施，离线可测）

| # | 检查项 | 方法 | 通过标准 |
|---|--------|------|---------|
| A1 | Python 语法 | `python -m py_compile models/task.py models/__init__.py core/config.py core/task_manager.py` | 无 SyntaxError |
| A2 | 模型导入 | `python -c "from models.task import TaskType, SimpleVideoTask, CreativeVideoTask, ManuscriptVideoTask, AudioConfig, SubtitleStyle, ManuscriptParagraph"` | 无 ImportError |
| A3 | 模型序列化 | SimpleVideoTask(prompt="test", mode="t2v", duration=5).model_dump_json() → 合法 JSON | 含 task_type="simple" |
| A4 | 旧数据兼容 | 创建无 task_type 字段的 task_state.json → TaskManager.load() → 返回 CreativeVideoTask | 不抛异常，type=CREATIVE |
| A5 | config 工厂 | get_default_audio_config() → AudioConfig(enabled=True, voice="zh-CN-XiaoxiaoNeural") | 字段完整 |
| A6 | 依赖安装 | `pip install -r requirements.txt` | edge_tts 安装成功 |

### Batch B 验证（通用组件+流水线，离线+可选在线）

| # | 检查项 | 方法 | 通过标准 |
|---|--------|------|---------|
| B1 | 导入链 | `from core.api import AgnesImageAPI, AgnesVideoAPI, AgnesChatAPI` 等 | 全部无 ImportError |
| B2 | Screenwriter 迁移 | `grep -c "requests.post" core/screenwriter.py` | 0（已改用 AgnesChatAPI） |
| B3 | 日志前缀 | 检查各文件 logger 前缀是否符合 AGENTS.md 规范 | [AgnesImage]/[TTS]/[Subtitle]/[Compositor]/[Simple]/[Pipeline]/[Manuscript] |
| B4 | cues_to_srt | 输入虚拟 cues `{0.0: "Hello", 1.0: "world"}` → SRT | 合法 SRT 时间戳格式 |
| B5 | split_manuscript | 空文本→[] ; 短句→1段 ; 30字×10句→按5-12s拆分 ; 超长单句→接受 | 均不拆句子 |
| B6 | Pipeline 结构 | 三个类都有 async def run(self, state) 方法 | 签名一致 |
| B7 | 旧文件兼容 | `from core.image_generator import ImageGeneratorAgnesAPI` → 可用 | 别名保留 |

### Batch C 验证（集成+前端，需服务启动）

| # | 检查项 | 方法 | 通过标准 |
|---|--------|------|---------|
| C1 | 服务启动 | `python server.py` | 无报错，8765 端口监听 |
| C2 | GET / | curl localhost:8765 | 200, HTML 含三个 Tab |
| C3 | i18n | 浏览器切换 7 种语言 | 新增文案翻译不缺失 |
| C4 | POST /api/tasks/simple | curl -X POST -d '{...}' | {"ok":true,"task_id":"..."} |
| C5 | POST /api/tasks/creative | 同上 | 同上 |
| C6 | POST /api/tasks/manuscript | 同上 | 同上 |
| C7 | GET /api/tasks | curl | 列表含三种 task_type |
| C8 | GET /api/tasks/{id} | curl | 响应含 task_type |
| C9 | 简单视频 Tab | 切换模式 → 参考图/尾帧区显隐 | 正确联动 |
| C10 | 稿件 Tab | textarea + [预览拆分] 按钮 | 可交互 |
| C11 | 创意视频 Tab | 音频配置区 | 开关/下拉/滑块/字幕样式可见 |

---

## 三、智能路由判定流程

```
第 N 轮测试完成
       ↓
  ┌ 失败项分析 ───────────────────────────┐
  │                                        │
  │  是源码逻辑问题？                       │
  │  ├─ YES → 反馈给工程师                  │
  │  │   └─ 附带：批次 ID + 失败检查项编号  │
  │  │      + 错误信息 + 期望 vs 实际       │
  │  │                                      │
  │  是测试代码问题（断言错/mock不当）？      │
  │  ├─ YES → QA 自行修复                    │
  │  │                                      │
  │  全部通过？                              │
  │  └─ YES → ALL_PASS，通知主理人汇总       │
  └──────────────────────────────────────────┘
```

---

## 四、每批通过标准

| 批次 | 全部通过条件 |
|------|------------|
| Batch A | A1-A6 全部 ✓ |
| Batch B | B1-B7 全部 ✓ |
| Batch C | C1-C11 全部 ✓ |

每批最多 2 轮测试。2 轮后仍有失败 → 输出遗留问题清单，标注是否阻塞后续批次。

---

## 五、交付产物

QA 完成每批测试后，通过 SendMessage 向主理人汇报（不落盘文件）：

```
### QA 测试报告 — Batch X

**批次**：Batch X — [名称]
**轮次**：第 1 轮 / 第 2 轮
**判定**：ALL_PASS / 反馈工程师 / QA自修复

**验证清单**：
| 检查项 | 状态 |
|--------|------|
| X1: ... | ✓ |
| X2: ... | ✓ |
| ... | ... |

**发现的问题**（如适用）：...
**修复建议**（如适用）：...
**是否阻塞后续批次**：是 / 否
```

---

*版本：v2.0 | 日期：2025-06-14 | 执行模式：分批*
