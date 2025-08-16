<script setup>
import { ref, computed } from 'vue'

// 模拟用户数据
const users = ref([
  { id: 1, name: '张三', email: 'zhangsan@example.com' },
  { id: 2, name: '李四', email: 'lisi@example.com' },
  { id: 3, name: '王五', email: 'wangwu@example.com' },
  { id: 4, name: '赵六', email: 'zhaoliu@example.com' },
  { id: 5, name: '钱七', email: 'qianqi@example.com' },
  { id: 6, name: '孙八', email: 'sunba@example.com' },
  { id: 7, name: '周九', email: 'zhoujiu@example.com' },
  { id: 8, name: '吴十', email: 'wushi@example.com' },
  { id: 9, name: '郑十一', email: 'zhengshiyi@example.com' },
  { id: 10, name: '王十二', email: 'wangshier@example.com' }
])

// 搜索关键字
const searchKeyword = ref('')

// 过滤后的用户数据（不区分大小写）
const filteredUsers = computed(() => {
  if (!searchKeyword.value) {
    return users.value
  }
  
  const keyword = searchKeyword.value.toLowerCase()
  return users.value.filter(user => 
    user.name.toLowerCase().includes(keyword)
  )
})

// 清空搜索
function clearSearch() {
  searchKeyword.value = ''
}
</script>

<template>
  <div class="search-container">
    <div class="search-header">
      <h1>用户搜索过滤</h1>
      <p>按名字关键字过滤用户列表（不区分大小写）</p>
    </div>
    
    <div class="search-input-container">
      <input
        type="text"
        v-model="searchKeyword"
        placeholder="请输入要搜索的用户姓名..."
        class="search-input"
      >
      <button @click="clearSearch" class="clear-button" v-if="searchKeyword">
        清空
      </button>
    </div>
    
    <div class="search-results">
      <div class="results-header">
        <span>共找到 {{ filteredUsers.length }} 个用户</span>
      </div>
      
      <div class="user-list">
        <div 
          v-for="user in filteredUsers" 
          :key="user.id" 
          class="user-item"
        >
          <div class="user-info">
            <div class="user-name">{{ user.name }}</div>
            <div class="user-email">{{ user.email }}</div>
          </div>
        </div>
      </div>
      
      <div v-if="filteredUsers.length === 0" class="no-results">
        <p>没有找到匹配的用户</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>