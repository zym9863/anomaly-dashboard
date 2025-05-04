<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import MetricCard from './MetricCard.vue';
import ThresholdSettings from './ThresholdSettings.vue';
import { ElRow, ElCol, ElDivider } from 'element-plus';

// 定义监控指标类型
interface Metric {
  id: string;
  name: string;
  value: number;
  unit: string;
  threshold: number;
  data: number[];
  isAnomalous: boolean;
}

// 初始化监控指标
const metrics = ref<Metric[]>([
  {
    id: 'temperature',
    name: '温度',
    value: 25,
    unit: '°C',
    threshold: 30,
    data: Array(30).fill(0).map(() => Math.floor(Math.random() * 10) + 20),
    isAnomalous: false
  },
  {
    id: 'pressure',
    name: '压力',
    value: 100,
    unit: 'kPa',
    threshold: 150,
    data: Array(30).fill(0).map(() => Math.floor(Math.random() * 50) + 80),
    isAnomalous: false
  },
  {
    id: 'humidity',
    name: '湿度',
    value: 45,
    unit: '%',
    threshold: 70,
    data: Array(30).fill(0).map(() => Math.floor(Math.random() * 30) + 40),
    isAnomalous: false
  },
  {
    id: 'vibration',
    name: '振动',
    value: 2.5,
    unit: 'mm/s',
    threshold: 5,
    data: Array(30).fill(0).map(() => Math.random() * 4 + 1),
    isAnomalous: false
  }
]);

// 更新数据的定时器
let dataUpdateTimer: number | null = null;

// 模拟数据更新
const updateMetricData = () => {
  metrics.value.forEach(metric => {
    // 生成新的随机值
    let newValue: number;

    // 有10%的概率生成异常值（超过阈值）
    const isAnomalous = Math.random() < 0.1;

    if (isAnomalous) {
      // 生成超过阈值的值
      newValue = metric.threshold + Math.random() * (metric.threshold * 0.2);
    } else {
      // 生成正常范围内的值
      switch (metric.id) {
        case 'temperature':
          newValue = Math.floor(Math.random() * 10) + 20; // 20-30
          break;
        case 'pressure':
          newValue = Math.floor(Math.random() * 50) + 80; // 80-130
          break;
        case 'humidity':
          newValue = Math.floor(Math.random() * 30) + 40; // 40-70
          break;
        case 'vibration':
          newValue = Math.random() * 4 + 1; // 1-5
          break;
        default:
          newValue = metric.value;
      }
    }

    // 更新当前值
    metric.value = Number(newValue.toFixed(metric.id === 'vibration' ? 1 : 0));

    // 更新数据历史
    metric.data.push(metric.value);
    if (metric.data.length > 30) {
      metric.data.shift();
    }

    // 检查是否超过阈值
    metric.isAnomalous = metric.value > metric.threshold;
  });
};

// 更新阈值设置
const updateThreshold = (id: string, value: number) => {
  const metric = metrics.value.find(m => m.id === id);
  if (metric) {
    metric.threshold = value;
    // 立即检查是否超过阈值
    metric.isAnomalous = metric.value > metric.threshold;
  }
};

// 组件挂载时启动数据更新
onMounted(() => {
  dataUpdateTimer = window.setInterval(updateMetricData, 2000);
});

// 组件卸载时清除定时器
onUnmounted(() => {
  if (dataUpdateTimer !== null) {
    clearInterval(dataUpdateTimer);
  }
});
</script>

<template>
  <div class="dashboard">
    <div class="dashboard-header">
      <h1>系统监控仪表盘</h1>
      <div class="dashboard-status">
        <ElTag
          v-if="metrics.some(m => m.isAnomalous)"
          type="danger"
          effect="dark"
          class="status-tag"
        >
          系统异常
        </ElTag>
        <ElTag
          v-else
          type="success"
          effect="dark"
          class="status-tag"
        >
          系统正常
        </ElTag>
      </div>
    </div>
    <ElDivider />

    <ElRow :gutter="20">
      <ElCol :span="24">
        <ThresholdSettings
          :metrics="metrics"
          @update-threshold="updateThreshold"
        />
      </ElCol>
    </ElRow>

    <ElRow :gutter="24" class="metric-cards">
      <ElCol
        v-for="metric in metrics"
        :key="metric.id"
        :xs="24"
        :sm="12"
        :md="12"
        :lg="6"
      >
        <MetricCard
          :metric="metric"
        />
      </ElCol>
    </ElRow>
  </div>
</template>

<style scoped>
.dashboard {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 24px;
  border-radius: var(--border-radius);
  box-shadow: var(--card-shadow);
  background-color: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  animation: slideInUp 0.5s ease-out;
}

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

h1 {
  margin: 0;
  font-size: 28px;
  font-weight: 700;
  background: linear-gradient(45deg, var(--primary-color), var(--success-color));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  letter-spacing: -0.5px;
}

.dashboard-status {
  display: flex;
  align-items: center;
}

.status-tag {
  font-size: 14px;
  padding: 6px 12px;
  border-radius: 20px;
  font-weight: 600;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.5s ease-in-out;
}

.metric-cards {
  margin-top: 24px;
}

/* 暗色模式支持 */
@media (prefers-color-scheme: dark) {
  .dashboard {
    background-color: rgba(30, 30, 30, 0.8);
    box-shadow: var(--card-shadow);
  }

  h1 {
    background: linear-gradient(45deg, #79bbff, #95d475);
    -webkit-background-clip: text;
    background-clip: text;
  }

  .status-tag {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  }
}
</style>
