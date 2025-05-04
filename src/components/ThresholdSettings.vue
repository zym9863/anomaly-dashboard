<script setup lang="ts">
import { ref } from 'vue';
import { ElCard, ElForm, ElFormItem, ElSlider, ElInputNumber, ElRow, ElCol } from 'element-plus';

// 定义组件属性
defineProps<{
  metrics: {
    id: string;
    name: string;
    value: number;
    unit: string;
    threshold: number;
    data: number[];
    isAnomalous: boolean;
  }[]
}>();

// 定义事件
const emit = defineEmits<{
  (e: 'updateThreshold', id: string, value: number): void
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

// 阈值更新处理函数
const handleThresholdChange = (id: string, value: number) => {
  emit('updateThreshold', id, value);
};

// 是否显示设置面板
const showSettings = ref(true);

// 切换设置面板显示状态
const toggleSettings = () => {
  showSettings.value = !showSettings.value;
};
</script>

<template>
  <ElCard class="threshold-settings" :shadow="'hover'">
    <template #header>
      <div class="settings-header">
        <div class="header-title">
          <i class="settings-icon">⚙️</i>
          <span>阈值设置</span>
        </div>
        <button class="toggle-btn" @click="toggleSettings">
          {{ showSettings ? '隐藏设置' : '显示设置' }}
        </button>
      </div>
    </template>

    <div v-if="showSettings" class="settings-content">
      <ElForm>
        <ElRow :gutter="24">
          <ElCol
            v-for="metric in metrics"
            :key="metric.id"
            :xs="24"
            :sm="12"
            :md="12"
            :lg="6"
          >
            <ElFormItem>
              <template #label>
                <div class="form-item-label">
                  <div class="metric-icon" :class="metric.id">
                    <i class="icon" :class="getIconClass(metric.id)"></i>
                  </div>
                  <span>{{ metric.name }}阈值</span>
                </div>
              </template>
              <div class="threshold-control">
                <ElSlider
                  v-model="metric.threshold"
                  :min="metric.id === 'temperature' ? 20 : (metric.id === 'pressure' ? 80 : (metric.id === 'humidity' ? 40 : 1))"
                  :max="metric.id === 'temperature' ? 40 : (metric.id === 'pressure' ? 200 : (metric.id === 'humidity' ? 100 : 10))"
                  :step="metric.id === 'vibration' ? 0.1 : 1"
                  :show-tooltip="true"
                  :format-tooltip="(val) => `${val}${metric.unit}`"
                  @change="(val) => handleThresholdChange(metric.id, val as number)"
                />
                <div class="input-container">
                  <ElInputNumber
                    v-model="metric.threshold"
                    :min="metric.id === 'temperature' ? 20 : (metric.id === 'pressure' ? 80 : (metric.id === 'humidity' ? 40 : 1))"
                    :max="metric.id === 'temperature' ? 40 : (metric.id === 'pressure' ? 200 : (metric.id === 'humidity' ? 100 : 10))"
                    :step="metric.id === 'vibration' ? 0.1 : 1"
                    :precision="metric.id === 'vibration' ? 1 : 0"
                    :controls-position="'right'"
                    @change="(val) => handleThresholdChange(metric.id, val as number)"
                  />
                  <span class="unit">{{ metric.unit }}</span>
                </div>
              </div>
              <div class="current-value" :class="{ 'anomalous': metric.isAnomalous }">
                当前值: {{ metric.value }}{{ metric.unit }}
                <span v-if="metric.isAnomalous" class="warning-icon">⚠️</span>
              </div>
            </ElFormItem>
          </ElCol>
        </ElRow>
      </ElForm>
    </div>
  </ElCard>
</template>


<style scoped>
.threshold-settings {
  margin-bottom: 24px;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.settings-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title {
  display: flex;
  align-items: center;
  font-size: 16px;
  font-weight: 600;
}

.settings-icon {
  margin-right: 8px;
  font-size: 18px;
}

.toggle-btn {
  background-color: #f0f9eb;
  border: 1px solid #e1f3d8;
  border-radius: 4px;
  color: #67C23A;
  cursor: pointer;
  font-size: 14px;
  padding: 6px 12px;
  transition: all 0.3s;
}

.toggle-btn:hover {
  background-color: #e1f3d8;
}

.settings-content {
  transition: all 0.3s ease;
  padding-top: 8px;
}

.form-item-label {
  display: flex;
  align-items: center;
}

.metric-icon {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 8px;
  color: white;
  font-size: 12px;
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

/* Element Plus icons (using pseudo-elements for demonstration) */
.el-icon-thermometer::before { content: '🌡️'; }
.el-icon-odometer::before { content: '⏱️'; }
.el-icon-water-cup::before { content: '💧'; }
.el-icon-mobile::before { content: '📳'; }
.el-icon-data-line::before { content: '📊'; }

.threshold-control {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 8px;
}

.threshold-control .el-slider {
  flex: 1;
}

.input-container {
  display: flex;
  align-items: center;
  width: 120px;
}

.threshold-control .el-input-number {
  width: 90px;
}

.unit {
  margin-left: 4px;
  color: #909399;
}

.current-value {
  font-size: 12px;
  color: #67C23A;
  text-align: right;
  padding-right: 8px;
}

.current-value.anomalous {
  color: #F56C6C;
  font-weight: 500;
}

.warning-icon {
  margin-left: 4px;
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

/* 暗色模式支持 */
@media (prefers-color-scheme: dark) {
  .toggle-btn {
    background-color: rgba(103, 194, 58, 0.1);
    border-color: rgba(103, 194, 58, 0.2);
  }

  .toggle-btn:hover {
    background-color: rgba(103, 194, 58, 0.2);
  }

  .unit, .current-value {
    color: #b0b3b8;
  }

  .current-value.anomalous {
    color: #F56C6C;
  }
}
</style>
