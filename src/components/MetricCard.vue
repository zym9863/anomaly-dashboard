<script setup lang="ts">
import { ref, onMounted, watch, onUnmounted } from 'vue';
import * as echarts from 'echarts';
import { ElCard, ElTag } from 'element-plus';

// 定义组件属性
const props = defineProps<{
  metric: {
    id: string;
    name: string;
    value: number;
    unit: string;
    threshold: number;
    data: number[];
    isAnomalous: boolean;
  }
}>();

// 获取指标对应的图标
const getIconClass = (id: string) => {
  switch (id) {
    case 'temperature':
      return 'el-icon-thermometer';
    case 'pressure':
      return 'el-icon-odometer';
    case 'humidity':
      return 'el-icon-water-cup';
    case 'vibration':
      return 'el-icon-mobile';
    default:
      return 'el-icon-data-line';
  }
};

// 图表实例引用
const chartRef = ref<HTMLElement | null>(null);
let chart: echarts.ECharts | null = null;

// 初始化图表
const initChart = () => {
  if (chartRef.value) {
    chart = echarts.init(chartRef.value);
    updateChart();
  }
};

// 更新图表数据
const updateChart = () => {
  if (!chart) return;

  const option = {
    grid: {
      top: 10,
      right: 10,
      bottom: 20,
      left: 40,
    },
    xAxis: {
      type: 'category',
      data: Array.from({ length: props.metric.data.length }, (_, i) => i + 1),
      axisLabel: {
        show: false
      }
    },
    yAxis: {
      type: 'value',
      min: (value: { min: number }) => Math.floor(value.min * 0.9),
      max: (value: { max: number }) => Math.ceil(value.max * 1.1),
    },
    series: [
      {
        data: props.metric.data,
        type: 'line',
        smooth: true,
        lineStyle: {
          color: props.metric.isAnomalous ? '#F56C6C' : '#67C23A',
          width: 3
        },
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            {
              offset: 0,
              color: props.metric.isAnomalous ? 'rgba(245, 108, 108, 0.5)' : 'rgba(103, 194, 58, 0.5)'
            },
            {
              offset: 1,
              color: props.metric.isAnomalous ? 'rgba(245, 108, 108, 0.1)' : 'rgba(103, 194, 58, 0.1)'
            }
          ])
        },
        markLine: {
          silent: true,
          lineStyle: {
            color: '#E6A23C',
            type: 'dashed'
          },
          data: [
            {
              yAxis: props.metric.threshold,
              label: {
                formatter: '阈值: {c}'
              }
            }
          ]
        }
      }
    ],
    tooltip: {
      trigger: 'axis'
    },
    animation: true
  };

  chart.setOption(option);
};

// 监听数据变化，更新图表
watch(() => props.metric.data, updateChart, { deep: true });
watch(() => props.metric.threshold, updateChart);
watch(() => props.metric.isAnomalous, updateChart);

// 监听窗口大小变化，调整图表大小
const handleResize = () => {
  chart?.resize();
};

// 组件挂载时初始化图表
onMounted(() => {
  initChart();
  window.addEventListener('resize', handleResize);
});

// 组件卸载时销毁图表
onUnmounted(() => {
  chart?.dispose();
  window.removeEventListener('resize', handleResize);
});
</script>

<template>
  <ElCard
    :class="['metric-card', { 'anomalous': metric.isAnomalous }]"
    :shadow="metric.isAnomalous ? 'always' : 'hover'"
  >
    <div class="card-header">
      <div class="metric-icon-container">
        <div class="metric-icon" :class="metric.id">
          <i class="icon" :class="getIconClass(metric.id)"></i>
        </div>
        <h3>{{ metric.name }}</h3>
      </div>
      <ElTag
        :type="metric.isAnomalous ? 'danger' : 'success'"
        effect="dark"
        class="status-tag"
      >
        {{ metric.isAnomalous ? '异常' : '正常' }}
      </ElTag>
    </div>

    <div class="metric-value-container">
      <div class="metric-value" :class="{ 'anomalous': metric.isAnomalous }">
        {{ metric.value }} <span class="unit">{{ metric.unit }}</span>
      </div>

      <div class="threshold-info">
        <span class="threshold-label">阈值:</span>
        <span class="threshold-value">{{ metric.threshold }} {{ metric.unit }}</span>
      </div>
    </div>

    <div ref="chartRef" class="chart-container"></div>
  </ElCard>
</template>


<style scoped>
.metric-card {
  margin-bottom: 24px;
  transition: all 0.3s ease;
  border-radius: 12px;
  overflow: hidden;
  height: 100%;
}

.metric-card.anomalous {
  border: 2px solid #F56C6C;
  animation: pulse 2s infinite;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.metric-icon-container {
  display: flex;
  align-items: center;
}

.metric-icon {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 12px;
  color: white;
}

.metric-icon.temperature {
  background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
}

.metric-icon.pressure {
  background: linear-gradient(135deg, #a1c4fd 0%, #c2e9fb 100%);
}

.metric-icon.humidity {
  background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
}

.metric-icon.vibration {
  background: linear-gradient(135deg, #fccb90 0%, #d57eeb 100%);
}

.icon {
  font-size: 18px;
}

/* Element Plus icons (using pseudo-elements for demonstration) */
.el-icon-thermometer::before { content: '🌡️'; }
.el-icon-odometer::before { content: '⏱️'; }
.el-icon-water-cup::before { content: '💧'; }
.el-icon-mobile::before { content: '📳'; }
.el-icon-data-line::before { content: '📊'; }

.card-header h3 {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
}

.status-tag {
  padding: 4px 8px;
  border-radius: 12px;
}

.metric-value-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 16px;
}

.metric-value {
  font-size: 32px;
  font-weight: bold;
  color: #67C23A;
  line-height: 1;
}

.metric-value.anomalous {
  color: #F56C6C;
}

.unit {
  font-size: 16px;
  font-weight: normal;
  opacity: 0.8;
}

.threshold-info {
  font-size: 14px;
  color: #909399;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

.threshold-label {
  font-size: 12px;
  opacity: 0.7;
}

.threshold-value {
  font-weight: 500;
}

.chart-container {
  height: 150px;
  width: 100%;
  margin-top: 8px;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0 rgba(245, 108, 108, 0.7);
  }
  50% {
    box-shadow: 0 0 0 10px rgba(245, 108, 108, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(245, 108, 108, 0);
  }
}

/* 暗色模式支持 */
@media (prefers-color-scheme: dark) {
  .threshold-info {
    color: #b0b3b8;
  }

  .metric-icon {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  }
}
</style>
