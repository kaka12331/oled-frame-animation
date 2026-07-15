# OLED 帧动画演示 — 月新喵

基于 **STM32F103C8** 的 **OLED 多帧位图动画** 示例。循环播放约 28 帧「月新喵」动画，并使用一系列 **显示优化技巧** 提升刷新速度、降低 I2C 通信量。

---

## 功能概述

- 帧数据存放于 `Hardware/animation.c`（位图数组）  
- 自定义 `OLED_ShowFrame_Magic`：居中裁剪显示、减半无效写入  
- 用位移/掩码替代除法取余，加快位图缩放映射  

### 提速要点（`main.c`）

1. 跳过两侧黑边，光标直接定位到居中起点（如 X=32）  
2. 每页只写 64 列有效数据，I2C 通信量约减半  
3. 坐标映射用 `>>` 代替除法  
4. 位索引用 `& 0x07` 代替 `% 8`  

---

## 目录说明

当前仓库主文件为源码压缩包；完整 Keil 工程结构通常为：

```
├── User/main.c              # 动画主循环
├── Hardware/
│   ├── animation.c / .h     # 帧位图数据
│   ├── OLED.*
│   └── Key / LED
├── System/ Library/ Start/
└── Project.uvprojx
```

可将压缩包解压后用 Keil 打开工程编译下载。

---

## 硬件

- STM32F103C8  
- 0.96″ OLED（驱动见工程 `OLED.c`，常见 I2C）  

---

## 相关仓库

- [2023-dian-sai-visual-gimbal](https://github.com/kaka12331/2023-dian-sai-visual-gimbal)  
- [dual-stepper-trapezoid](https://github.com/kaka12331/dual-stepper-trapezoid)  

用于学习 OLED 刷新优化与趣味演示。
