<script setup lang="ts">
import Dashboard from './components/Dashboard.vue'
import 'element-plus/dist/index.css'
</script>

<template>
  <div class="app-container">
    <Dashboard />
  </div>
</template>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: 'Nunito', 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', Arial, sans-serif;
  background-color: var(--bg-color);
  color: var(--text-color);
  background-image: linear-gradient(to right, rgba(235, 238, 245, 0.5) 1px, transparent 1px),
                    linear-gradient(to bottom, rgba(235, 238, 245, 0.5) 1px, transparent 1px);
  background-size: 20px 20px;
}

.app-container {
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 20px 0;
  animation: fadeIn 0.5s ease-in-out;
}

/* 暗色模式支持 */
@media (prefers-color-scheme: dark) {
  body {
    background-image: linear-gradient(to right, rgba(50, 50, 50, 0.3) 1px, transparent 1px),
                      linear-gradient(to bottom, rgba(50, 50, 50, 0.3) 1px, transparent 1px);
  }
}
</style>
