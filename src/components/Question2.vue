<script setup>
import { ref, computed } from 'vue'

// 生成50条模拟数据
const generateMockData = () => {
  const data = []
  for (let i = 1; i <= 50; i++) {
    data.push({
      id: i,
      name: `用户${i}`,
      age: Math.floor(Math.random() * 50) + 18,
      address: `地址${i}号`,
      email: `user${i}@example.com`
    })
  }
  return data
}

// 模拟数据
const mockData = ref(generateMockData())

// 分页相关状态
const currentPage = ref(1)
const pageSize = ref(10)

// 计算总页数
const totalPages = computed(() => {
  return Math.ceil(mockData.value.length / pageSize.value)
})

// 计算当前页数据
const currentPageData = computed(() => {
  const startIndex = (currentPage.value - 1) * pageSize.value
  const endIndex = startIndex + pageSize.value
  return mockData.value.slice(startIndex, endIndex)
})

// 上一页
function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}

// 下一页
function nextPage() {
  if (currentPage.value < totalPages.value) {
    currentPage.value++
  }
}

// 跳转到指定页
function goToPage(page) {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
  }
}
</script>

<template>
  <div class="pagination-container">
    <div class="pagination-header">
      <h1>分页数据展示</h1>
      <p>共 {{ mockData.length }} 条记录，每页显示 {{ pageSize }} 条</p>
    </div>
    
    <div class="data-table-container">
      <table class="data-table">
        <thead>
          <tr>
            <th>ID</th>
            <th>姓名</th>
            <th>年龄</th>
            <th>地址</th>
            <th>邮箱</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in currentPageData" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.age }}</td>
            <td>{{ item.address }}</td>
            <td>{{ item.email }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    
    <div class="pagination-controls">
      <button @click="prevPage" :disabled="currentPage === 1">上一页</button>
      
      <div class="page-numbers">
        <button
          v-for="page in totalPages"
          :key="page"
          @click="goToPage(page)"
          :class="{ active: currentPage === page }"
        >
          {{ page }}
        </button>
      </div>
      
      <button @click="nextPage" :disabled="currentPage === totalPages">下一页</button>
    </div>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>