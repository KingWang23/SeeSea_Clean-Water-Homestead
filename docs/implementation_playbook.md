# 实施手册（早期方案参考）

> 说明：本文件保留早期黑客马拉松设计思路，仅作参考。当前可运行系统请以 `docs/usage_guide.md`、`docs/mobile_access_guide.md` 和 `docs/system_ppt_brief.md` 为准。

## A. 目标拆解

1. 图像增强：让水下图像可辨识、可用于后续检测。
2. 垃圾身份证：输出“类别/材质/来源/数量/状态”。
3. 表单核对：自动发现漏填、错填、数量偏差。
4. 协作平台：网页展示 + 小程序提交 + 后台分析联动。
5. 反馈语义：提取密集度、复杂度、安全风险。
6. 沉浸式展示：在浏览器完成 3D 场景演示。

## B. 系统架构

- 后端：FastAPI + SQLAlchemy + SQLite
- AI 服务层：
  - `enhancer.py` 图像增强
  - `identifier.py` 图像/OCR/包装匹配融合识别
  - `reconcile.py` 表单核对
  - `trace.py` 溯源轨迹模拟
  - `feedback_nlp.py` 文本语义评分
- 前端：
  - Web 页面（Leaflet 地图 + 数据面板 + Three.js）
  - H5 小程序模拟页
  - 微信小程序模板代码（`miniprogram/`）

## C. 关键流程（端到端）

1. 潜水员上传图片到 `/api/records/{id}/media`
2. 增强模块执行 `/api/records/{id}/enhance`
3. 识别模块执行 `/api/records/{id}/identify`
4. 人工表单提交 `/api/records/{id}/manual-form`
5. 自动核对 `/api/records/{id}/reconcile`
6. 溯源模拟 `/api/records/{id}/trace`
7. 反馈分析 `/api/feedback/analyze`
8. Web 拉取汇总 `/api/dashboard/summary` 与 `/api/immersive-data`

## D. 比赛期间建议分工

1. AI组：增强 + 检测 + OCR + 融合策略。
2. 平台组：后端 API + 数据库 + 可视化看板。
3. 小程序组：采集端表单/图片上传 + 联调。
4. 数据组：标注规范、模板库维护、评测指标。

## E. 可交付 Demo 标准

1. 能上传图片并看到增强前后差异（清晰度指标提升）。
2. 自动输出至少 3 类垃圾的“身份证”字段。
3. 能生成“已/待清理”清单并和人工表单核对。
4. 地图上可按年份显示潜点。
5. 可展示一条溯源路径模拟线。
6. 可生成一段反馈语义分析报告。
7. 3D页面可显示垃圾热点点云/球体分布。

## F. 赛后升级路线（建议）

1. 将 `identifier.py` 替换为微调 YOLOv8/YOLO11 + PaddleOCR。
2. 将 `trace.py` 替换为 OpenDrift + 实时海流数据。
3. 将 `feedback_nlp.py` 升级为中文 SBERT + 分类器。
4. 数据库从 SQLite 升级到 PostgreSQL + PostGIS。
5. 可视化升级为 MapLibre/Cesium + 时空动画。
