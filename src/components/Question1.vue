<script setup>
import { ref } from 'vue'

// 模拟任务数据
const tasks = ref([
  { id: 1, title: '完成项目文档', status: 'pending' },
  { id: 2, title: '修复首页的样式问题', status: 'completed' },
  { id: 3, title: '优化性能问题', status: 'pending' },
  { id: 4, title: '添加新功能', status: 'pending' },
  { id: 5, title: '代码审查', status: 'completed' }
])

// 切换任务状态
function toggleTaskStatus(taskId) {
  const task = tasks.value.find(t => t.id === taskId)
  if (task) {
    task.status = task.status === 'completed' ? 'pending' : 'completed'
  }
}
</script>

<template>
  <div class="task-container">
    <div class="task-header">
      <h1>任务列表</h1>
    </div>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" class="task-item">
        <div class="task-info">
          <div class="task-title">{{ task.title }}</div>
          <div class="task-status" :class="task.status">
            {{ task.status === 'completed' ? '已完成' : '未完成' }}
          </div>
        </div>
        <button @click="toggleTaskStatus(task.id)">
          {{ task.status === 'completed' ? '标记为未完成' : '标记为已完成' }}
        </button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>