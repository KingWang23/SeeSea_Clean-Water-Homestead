# SeeSea 洁净水下家园
<span style="color: red;">**代码会全部开源！！！**</span>

SeeSea 是一套面向海洋垃圾治理行动的双端智能协作系统，用于把潜水员现场采集的水下图像、人工表单和志愿者反馈，转化为可识别、可核对、可追踪、可导出的治理数据。

系统核心定位：

- 潜水员端：移动端拍照/上传、填写位置和表单、提交现场案例
- 管理员端：查看总览、审核案例、生成报告、导出数据、跟踪潜点态势
- AI 分析链路：图像增强、垃圾识别、OCR/包装线索、表单核对、语义分析、扩散路径模拟、案例报告、指挥简报
- 降级容错：远程 LLM 不可用时可回退到本地规则，保证系统仍可运行和演示

## 1. 快速开始

### 1.1 环境启动

```bash
cd <project-root>
conda activate clean_underwater_demo
cp .env.example .env
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

说明：

- `<project-root>` 替换为当前项目根目录
- 长时间联调建议不要使用 `--reload`，可减少 SQLite 锁冲突和退出卡顿
- 本机访问：`http://127.0.0.1:8000/`
- 手机访问：`http://你的电脑局域网IP:8000/`
- 手机浏览器访问时必须使用 `http://`，不要使用 `https://`

### 1.2 默认账号

- 管理员：`admin / admin123`
- 潜水员 A：`diver_a / diver123`
- 潜水员 B：`diver_b / diver123`

### 1.3 基本流程

1. 管理员或潜水员从登录页进入系统
2. 潜水员通过手机拍照或上传图片，填写位置、表单和反馈后提交案例
3. 系统自动执行图像增强、垃圾识别、表单核对、语义分析和扩散模拟
4. 管理员在后台查看案例详情、地图总览、风险提醒和 AI 指挥简报
5. 管理员可更新审核状态、导出数据、复盘潜点情况

## 2. 系统能力概览

### 2.1 潜水员端

- 手机拍照上传或相册选择
- 图片预览
- 浏览器定位 + 地图选点双模式
- 人工表单条目录入
- 志愿者语义反馈录入
- 提交后查看原图、增强图、识别图和分析结果
- 删除本人案例

### 2.2 管理员端

- 看板指标、地图、趋势、来源分布
- AI 指挥简报
- 风险预警和运营提醒
- 案例详情查看
- 识别与人工核对对比
- 工作流状态维护
- 新增潜水员账号
- Markdown / JSON / CSV 导出
- LLM 连通性测试

### 2.3 AI 与分析能力

- 水下图像增强
- 传统视觉规则识别
- 可选本地 ONNX 检测器融合
- 可接入 SiliconFlow 视觉模型
- OCR 与包装线索辅助判断
- 人工表单自动核对
- 志愿者反馈语义分析
- 扩散路径规则模拟
- 单案例报告与管理员简报生成

## 3. SiliconFlow 配置

系统默认使用 SiliconFlow：

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

- 未配置 `SILICONFLOW_API_KEY` 时，系统会自动降级为本地规则
- `SILICONFLOW_TIMEOUT_SECONDS` 控制超时
- `SILICONFLOW_MAX_RETRIES` 控制重试次数
- `SILICONFLOW_MAX_TOKENS` 控制单次输出长度
- 当前项目默认使用 `.cn` 网关，并保留 `.com -> .cn` 的兼容补试逻辑
- 若配置 `LOCAL_DETECTOR_ONNX_PATH` 和 `LOCAL_DETECTOR_LABELS_PATH`，系统会融合本地检测器结果

## 4. 推荐文档阅读顺序

- 快速上手：[docs/usage_guide.md](./docs/usage_guide.md)
- 详细功能说明：[docs/feature_specification.md](./docs/feature_specification.md)
- 详细使用说明：[docs/detailed_usage_manual.md](./docs/detailed_usage_manual.md)
- 完整技术报告：[docs/full_technical_report.md](./docs/full_technical_report.md)
- 手机访问说明：[docs/mobile_access_guide.md](./docs/mobile_access_guide.md)
- PPT 讲解提纲：[docs/system_ppt_brief.md](./docs/system_ppt_brief.md)
- 论文与项目参考：[docs/papers_and_projects.md](./docs/papers_and_projects.md)
- 早期实施手册归档：[docs/implementation_playbook.md](./docs/implementation_playbook.md)

## 5. 核心代码入口

- 后端入口：`app/main.py`
- 数据模型：`app/models.py`
- 图像增强：`app/services/enhancer.py`
- 垃圾识别融合：`app/services/identifier.py`
- 本地检测器：`app/services/local_detector.py`
- 报告与评估：`app/services/reporting.py`
- SiliconFlow 接口：`app/services/siliconflow.py`
- 管理员模板：`app/templates/admin.html`
- 潜水员模板：`app/templates/diver.html`

## 6. 测试与验证

```bash
conda run -n clean_underwater_demo env PYTEST_DISABLE_PLUGIN_AUTOLOAD=1 pytest -q
```

如果要验证服务是否能正常导入：

```bash
conda run -n clean_underwater_demo python -c "from app.main import app; print('startup_ok')"
```

