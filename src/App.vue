<script setup>
import { ref } from 'vue'
import Question1 from './components/Question1.vue'
import Question2 from './components/Question2.vue'
import Question3 from './components/Question3.vue'
import Question4 from './components/Question4.vue'
import Question5 from './components/Question5.vue'
import Question6 from './components/Question6.vue'
import Question7 from './components/Question7.vue'

// 模拟题目数据
const questions = [
  { id: 1, title: '1、简单任务列表页面' },
  { id: 2, title: '2、分页数据展示' },
  { id: 3, title: '3、搜索过滤功能' },
  { id: 4, title: '4、表单输入与校验' },
  { id: 5, title: '5、模态弹窗组件' },
  { id: 6, title: '6、文件上传预览' },
  { id: 7, title: '7、简单图表展示' }
]

// 当前选中的题目
const selectedQuestion = ref(1)
</script>

<template>
  <div class="app-container">
    <!-- 左侧菜单 -->
    <div class="menu-container">
      <h2 class="menu-title">题目列表</h2>
      <div 
        v-for="question in questions" 
        :key="question.id"
        class="menu-item"
        :class="{ active: selectedQuestion === question.id }"
        @click="selectedQuestion = question.id"
      >
        {{ question.title }}
      </div>
    </div>
    
    <!-- 右侧内容区域 -->
    <div class="content-container">
      <!-- 题目1：任务列表页面 -->
      <Question1 v-if="selectedQuestion === 1" />
      
      <!-- 题目2：分页数据展示 -->
      <Question2 v-else-if="selectedQuestion === 2" />
      
      <!-- 题目3 -->
      <Question3 v-else-if="selectedQuestion === 3" />
      
      <!-- 题目4 -->
      <Question4 v-else-if="selectedQuestion === 4" />
      
      <!-- 题目5 -->
      <Question5 v-else-if="selectedQuestion === 5" />
      
      <!-- 题目6 -->
      <Question6 v-else-if="selectedQuestion === 6" />
      
      <!-- 题目7 -->
      <Question7 v-else-if="selectedQuestion === 7" />
    </div>
  </div>
</template>

<style scoped>
/* 样式已经在 style.css 中定义 */
</style>
