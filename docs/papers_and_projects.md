# 论文与开源项目参考

本文件用于说明 SeeSea 后续模型升级、技术论证和答辩材料可引用的方向。

## 1. 水下图像增强

- Water-Net / UIEB  
  https://arxiv.org/abs/1901.05495
- RUIE 数据集  
  https://arxiv.org/abs/1901.05320
- FUnIE-GAN  
  https://arxiv.org/abs/1903.09766
- FUnIE-GAN 项目  
  https://github.com/xahidbuffon/FUnIE-GAN

SeeSea 中的作用：

- 对水下偏色、低对比、模糊图像进行增强。
- 为 YOLO、OCR 和 VLM 提供更高质量输入。
- 在垃圾身份证中展示增强图，辅助人工复核。

## 2. 垃圾检测与视觉识别

- YOLO 系列目标检测  
  https://arxiv.org/abs/1506.02640
- Ultralytics YOLO 文档  
  https://docs.ultralytics.com/
- TACO 垃圾数据集  
  https://arxiv.org/abs/2003.06975
- CLIP 图文对齐  
  https://arxiv.org/abs/2103.00020

SeeSea 中的作用：

- YOLO 用于垃圾检测框和数量估计。
- VLM 用于结合图片、OCR、现场备注进行语义识别。
- 检测结果、分类结果和人工复核结果共同形成 AI 证据链。

## 3. OCR 与包装线索识别

- CRAFT 文本检测  
  https://arxiv.org/abs/1904.01941
- PaddleOCR  
  https://github.com/PaddlePaddle/PaddleOCR

SeeSea 中的作用：

- 从包装、瓶身、标签中提取文字线索。
- 辅助判断垃圾类别、品牌线索和可能来源活动。

## 4. 漂流模拟与空间分析

- OpenDrift 文档  
  https://opendrift.github.io/
- OpenDrift 论文  
  https://doi.org/10.5194/gmd-11-1405-2018
- OpenDrift GitHub  
  https://github.com/opendrift
- PostGIS  
  https://postgis.net/

SeeSea 中的作用：

- PostGIS 管理垃圾点、潜点、任务点和区域空间查询。
- OpenDrift / Lagrangian particle tracking 用于海洋环境数据驱动的漂流分析。
- 后续可接入海流、潮汐、风速风向、浪高和海岸线数据。

## 5. 平台技术

- FastAPI  
  https://fastapi.tiangolo.com/
- SQLAlchemy  
  https://docs.sqlalchemy.org/
- PostgreSQL  
  https://www.postgresql.org/

SeeSea 中的作用：

- FastAPI 提供后端 API。
- PostgreSQL + PostGIS 提供结构化和空间数据底座。
- SQLAlchemy 负责数据库访问。

## 6. 后续升级路线

1. 使用真实水下数据微调图像增强模型。
2. 使用人工审核样本训练水下垃圾检测模型。
3. 接入 OCR 和 VLM 多模型融合。
4. 接入真实海洋环境 reader，提升 OpenDrift 可信度。
5. 将 Marine Debris Knowledge Graph 从关系表升级为图数据库或 RDF 表示。
6. 将报告中心升级为可配置多智能体工作流。
