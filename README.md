# Anomaly Dashboard

[English](README.md) | [中文](README_zh.md)

A real-time monitoring dashboard built with Vue 3, TypeScript, and Element Plus that visualizes system metrics and detects anomalies.

## Overview

Anomaly Dashboard is a modern web application designed to monitor critical system metrics in real-time. It provides visual alerts when metrics exceed predefined thresholds, helping operators quickly identify and respond to potential issues.

## Features

- **Real-time Monitoring**: Continuously updates metrics every 2 seconds
- **Anomaly Detection**: Visual alerts when metrics exceed thresholds
- **Customizable Thresholds**: Adjust threshold values through an intuitive interface
- **Interactive Charts**: Visualize metric trends with dynamic line charts
- **Responsive Design**: Optimized for both desktop and mobile devices
- **Dark Mode Support**: Automatic theme switching based on system preferences

## Metrics Monitored

The dashboard currently tracks the following metrics:

- **Temperature**: Measured in °C
- **Pressure**: Measured in kPa
- **Humidity**: Measured in %
- **Vibration**: Measured in mm/s

## Technologies Used

- **Vue 3**: Frontend framework with Composition API
- **TypeScript**: Type-safe JavaScript
- **Vite**: Next-generation frontend build tool
- **Element Plus**: UI component library
- **ECharts**: Interactive charting library
- **CSS Variables**: For theming and dark mode support

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn

### Installation

```bash
# Clone the repository
git clone https://github.com/zym9863/anomaly-dashboard.git

# Navigate to the project directory
cd anomaly-dashboard

# Install dependencies
npm install
# or
yarn install
```

### Development

```bash
# Start the development server
npm run dev
# or
yarn dev
```

### Build for Production

```bash
# Build the project
npm run build
# or
yarn build
```

### Preview Production Build

```bash
# Preview the production build
npm run preview
# or
yarn preview
```

## Project Structure

```
anomaly-dashboard/
├── public/
├── src/
│   ├── components/
│   │   ├── Dashboard.vue       # Main dashboard component
│   │   ├── MetricCard.vue      # Individual metric display
│   │   └── ThresholdSettings.vue # Threshold configuration
│   ├── App.vue                 # Root component
│   ├── main.ts                 # Entry point
│   └── style.css               # Global styles
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

## License

[MIT](LICENSE)
