# SeeSea 洁净水下家园
<span style="color: red;">**代码会全部开源！！！**</span>

SeeSea 是一套面向海洋垃圾治理行动的双端系统：

- 潜水员端：手机拍照或上传图片，填写经纬度、人工表单、志愿者反馈
- 管理员端：案例审核、删除、状态流转、导出报告、潜水员管理、地图分析
- AI 识别：传统视觉规则 + 可选本地 ONNX 检测器 + SiliconFlow `THUDM/GLM-4.1V-9B-Thinking`
- LLM 分析：SiliconFlow `deepseek-ai/DeepSeek-R1-Distill-Qwen-7B` 负责文本分析、数据总结和文案生成
- 指挥能力：管理员筛选后可生成 AI 指挥简报、同源污染预警、传播建议和数据质量提醒

## 启动

```bash
conda activate clean_underwater_demo
cp .env.example .env
uvicorn app.main:app --reload --port 8000
```

长时间联调或频繁调用 LLM 时，建议关闭热重载，减少 SQLite 锁冲突和退出卡顿：

```bash
uvicorn app.main:app --port 8000
```

入口：

- 登录页：`http://127.0.0.1:8000/`
- 潜水员端：`http://127.0.0.1:8000/diver`
- 管理员端：`http://127.0.0.1:8000/admin`

说明：

- 根路径 `/` 现在固定为登录页
- 若需要让手机访问，请使用：

```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## 默认账号

- 管理员：`admin / admin123`
- 潜水员 A：`diver_a / diver123`
- 潜水员 B：`diver_b / diver123`

## SiliconFlow 配置

系统已切换为 SiliconFlow：

```bash
APP_SESSION_SECRET=replace-with-a-random-secret
SILICONFLOW_API_KEY=你的 SiliconFlow Key
SILICONFLOW_BASE_URL=https://api.siliconflow.cn/v1
SILICONFLOW_VISION_MODEL=THUDM/GLM-4.1V-9B-Thinking
SILICONFLOW_TEXT_MODEL=deepseek-ai/DeepSeek-R1-Distill-Qwen-7B
SILICONFLOW_TIMEOUT_SECONDS=60
SILICONFLOW_MAX_RETRIES=2
SILICONFLOW_MAX_TOKENS=1200
LOCAL_DETECTOR_ONNX_PATH=
LOCAL_DETECTOR_LABELS_PATH=
```

说明：

- 若 `SILICONFLOW_API_KEY` 为空，系统自动降级为本地规则识别与本地规则报告。
- `SILICONFLOW_TIMEOUT_SECONDS` 可控制远程模型超时，默认 `60` 秒。
- `SILICONFLOW_MAX_RETRIES` 控制失败后的重试次数，默认 `2`。
- `SILICONFLOW_MAX_TOKENS` 控制单次输出上限，避免推理模型输出过长导致超时。
- 当前项目默认使用 `https://api.siliconflow.cn/v1`。系统内部保留 `.com -> .cn` 的自动补试逻辑，便于不同网络环境下兼容。
- 若配置 `LOCAL_DETECTOR_ONNX_PATH` 和 `LOCAL_DETECTOR_LABELS_PATH`，系统会额外启用本地 ONNX 检测器参与识别融合。
- 管理员端会展示当前视觉模型和文本模型状态。
- 管理员端与潜水员端已改为角色独立 token 鉴权，同一浏览器可分别登录两个角色，不再互相覆盖登录态。

## 新增能力

- 删除案例：潜水员可删除自己的案例，管理员可删除任意案例
- 删除清理：删除案例时会同步清理未被其他案例复用的上传图片、增强图和标注图
- 相似案例推荐：详情页自动给出相近潜点 / 相近来源 / 相近风险案例
- 证据链展示：每条 AI 识别会显示证据来源，例如 `cv-heuristic`、`local-dnn`、`glm-vision`
- 管理员工作流：审核状态、清理状态、优先级、管理员备注
- 导出：单案例 Markdown/JSON 导出，筛选后 CSV 导出
- 文案生成：自动生成志愿者行动简报、公众传播文案、创新建议
- 看板联动：管理员筛选条件会同步影响地图、统计图、列表和 AI 指挥简报

## 关键文件

- 后端入口：[app/main.py](./app/main.py)
- 数据模型：[app/models.py](/home/stf/比赛/CCF%20海洋马拉松/Project/app/models.py)
- SiliconFlow 接口：[app/services/siliconflow.py](.app/services/siliconflow.py)
- 垃圾识别融合：[app/services/identifier.py](./app/services/identifier.py)
- 本地 ONNX 检测器：[app/services/local_detector.py](./app/services/local_detector.py)
- 报告与导出：[app/services/reporting.py](./app/services/reporting.py)
- 管理员页：[app/templates/admin.html](./app/templates/admin.html)
- 潜水员页：[app/templates/diver.html](./app/templates/diver.html)

## 文档

- 使用教程：[docs/usage_guide.md](./docs/usage_guide.md)
- 详细功能说明：[docs/feature_specification.md](./docs/feature_specification.md)
- 详细使用说明：[docs/detailed_usage_manual.md](./docs/detailed_usage_manual.md)
- 完整技术报告：[docs/full_technical_report.md](./docs/full_technical_report.md)
- 手机访问说明：[docs/mobile_access_guide.md](./docs/mobile_access_guide.md)
- PPT 讲解提纲：[docs/system_ppt_brief.md](./docs/system_ppt_brief.md)
- 论文与项目参考：[docs/papers_and_projects.md](./docs/papers_and_projects.md)

## 测试

```bash
conda run -n clean_underwater_demo env PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 pytest -q
```
