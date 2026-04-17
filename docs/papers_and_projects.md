# 论文与开源项目参考

> 本文件用于答辩补充材料和赛后升级路线说明。当前 SeeSea 工程版本以“可运行、可演示、可扩展”为优先。

## 1. 水下图像增强

1. Water-Net / UIEB  
   https://arxiv.org/abs/1901.05495
2. RUIE 数据集  
   https://arxiv.org/abs/1901.05320
3. FUnIE-GAN  
   https://arxiv.org/abs/1903.09766
4. FUnIE-GAN 项目代码  
   https://github.com/xahidbuffon/FUnIE-GAN

## 2. 垃圾检测与识别

1. YOLO  
   https://arxiv.org/abs/1506.02640
2. TACO 垃圾数据集  
   https://arxiv.org/abs/2003.06975
3. CLIP 图文对齐  
   https://arxiv.org/abs/2103.00020
4. Ultralytics YOLO 文档  
   https://docs.ultralytics.com/

## 3. OCR 与文本识别

1. CRAFT 文本检测  
   https://arxiv.org/abs/1904.01941
2. PaddleOCR 技术报告  
   https://arxiv.org/abs/2507.05595
3. PaddleOCR 项目  
   https://github.com/PaddlePaddle/PaddleOCR

## 4. 扩散路径与漂移模拟

1. OpenDrift 文档  
   https://opendrift.github.io/
2. OpenDrift 论文  
   https://doi.org/10.5194/gmd-11-1405-2018
3. OpenDrift GitHub  
   https://github.com/opendrift

## 5. 平台与可视化技术

1. FastAPI 官方文档  
   https://fastapi.tiangolo.com/
2. Leaflet 官方网站  
   https://leafletjs.com/
3. SQLAlchemy 官方文档  
   https://docs.sqlalchemy.org/

## 6. 当前工程版与这些资料的关系

当前 SeeSea 工程版采用“可稳定演示优先”的方案：

- 图像增强使用工程化可运行流程
- 垃圾识别使用规则、多源融合和可选模型接口
- 扩散路径使用规则驱动模拟

这样做的原因是：

- 比赛现场更稳定
- 部署更轻量
- 后续更容易逐步替换为更强模型

## 7. 赛后升级建议

### 7.1 图像增强升级

- 将当前增强模块替换为 Water-Net / FUnIE-GAN 推理接口

### 7.2 检测升级

- 将当前识别模块逐步升级为 YOLO + PaddleOCR + CLIP 融合流程

### 7.3 扩散模拟升级

- 将当前规则轨迹升级为 OpenDrift + 海流与潮汐数据驱动

### 7.4 数据层升级

- SQLite 升级为 PostgreSQL / PostGIS
- 引入对象存储与异步任务队列
