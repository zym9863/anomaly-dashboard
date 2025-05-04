# 异常监控仪表盘

[English](README.md) | [中文](README_zh.md)

一个使用 Vue 3、TypeScript 和 Element Plus 构建的实时监控仪表盘，用于可视化系统指标并检测异常。

## 概述

异常监控仪表盘是一个现代化的 Web 应用程序，旨在实时监控关键系统指标。当指标超过预定义的阈值时，它会提供视觉警报，帮助操作人员快速识别并响应潜在问题。

## 功能特点

- **实时监控**：每 2 秒连续更新指标
- **异常检测**：当指标超过阈值时提供视觉警报
- **可自定义阈值**：通过直观的界面调整阈值
- **交互式图表**：使用动态折线图可视化指标趋势
- **响应式设计**：针对桌面和移动设备进行优化
- **深色模式支持**：根据系统偏好自动切换主题

## 监控指标

仪表盘目前跟踪以下指标：

- **温度**：以 °C 为单位
- **压力**：以 kPa 为单位
- **湿度**：以 % 为单位
- **振动**：以 mm/s 为单位

## 使用的技术

- **Vue 3**：前端框架，使用组合式 API
- **TypeScript**：类型安全的 JavaScript
- **Vite**：下一代前端构建工具
- **Element Plus**：UI 组件库
- **ECharts**：交互式图表库
- **CSS 变量**：用于主题和深色模式支持

## 快速开始

### 前提条件

- Node.js (v14 或更高版本)
- npm 或 yarn

### 安装

```bash
# 克隆仓库
git clone https://github.com/zym9863/anomaly-dashboard.git

# 进入项目目录
cd anomaly-dashboard

# 安装依赖
npm install
# 或
yarn install
```

### 开发

```bash
# 启动开发服务器
npm run dev
# 或
yarn dev
```

### 生产环境构建

```bash
# 构建项目
npm run build
# 或
yarn build
```

### 预览生产环境构建

```bash
# 预览生产环境构建
npm run preview
# 或
yarn preview
```

## 项目结构

```
anomaly-dashboard/
├── public/
├── src/
│   ├── components/
│   │   ├── Dashboard.vue       # 主仪表盘组件
│   │   ├── MetricCard.vue      # 单个指标显示
│   │   └── ThresholdSettings.vue # 阈值配置
│   ├── App.vue                 # 根组件
│   ├── main.ts                 # 入口点
│   └── style.css               # 全局样式
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

## 许可证

[MIT](LICENSE)
