# 论文与开源项目参考（答辩补充材料）

> 说明：本文件用于论文和开源项目引用。系统功能与流程说明请以 `docs/system_ppt_brief.md` 为主。

## 1) 水下图像增强/还原

1. UIEB + Water-Net（经典基准）  
   https://arxiv.org/abs/1901.05495
2. RUIE（真实场景增强基准）  
   https://arxiv.org/abs/1901.05320
3. FUnIE-GAN（实时增强）  
   https://arxiv.org/abs/1903.09766
4. FUnIE-GAN 项目代码  
   https://github.com/xahidbuffon/FUnIE-GAN

## 2) 垃圾识别/检测与多模态融合

1. YOLO（实时检测基础）  
   https://arxiv.org/abs/1506.02640
2. TACO（垃圾检测数据集）  
   https://arxiv.org/abs/2003.06975
3. CLIP（图文跨模态匹配）  
   https://arxiv.org/abs/2103.00020
4. Ultralytics YOLO 文档（训练/部署）  
   https://docs.ultralytics.com/

## 3) OCR 与文本抽取

1. CRAFT（场景文字检测）  
   https://arxiv.org/abs/1904.01941
2. PaddleOCR 3.0 技术报告（2025）  
   https://arxiv.org/abs/2507.05595
3. PaddleOCR 项目  
   https://github.com/PaddlePaddle/PaddleOCR

## 4) 溯源与扩散路径模拟

1. OpenDrift 文档（海洋漂移模拟框架）  
   https://opendrift.github.io/
2. OpenDrift 论文（GMD 2018）  
   https://doi.org/10.5194/gmd-11-1405-2018
3. OpenDrift GitHub  
   https://github.com/opendrift

## 5) 平台与可视化

1. FastAPI 官方文档  
   https://fastapi.tiangolo.com/
2. Leaflet（交互地图）  
   https://leafletjs.com/
3. Three.js（Web沉浸式3D）  
   https://threejs.org/docs/

## 6) 本项目如何映射这些论文

1. Demo 当前版本采用“工程可落地优先”：增强与识别用可运行规则/传统视觉流程，保证黑客马拉松稳定演示。
2. 赛后把 `app/services/enhancer.py` 替换为 FUnIE-GAN/Water-Net 推理接口。
3. 把 `app/services/identifier.py` 替换为 YOLO + PaddleOCR + CLIP 三路融合推理。
4. 把 `app/services/trace.py` 替换为 OpenDrift + 海流实况数据驱动。
