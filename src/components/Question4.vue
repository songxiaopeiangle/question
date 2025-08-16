<script setup>
import { ref } from 'vue'

// 表单数据
const formData = ref({
  username: '',
  email: '',
  password: ''
})

// 错误信息
const errors = ref({
  username: '',
  email: '',
  password: ''
})

// 提交状态
const isSubmitted = ref(false)

// 校验邮箱格式的正则表达式
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/

// 校验表单
function validateForm() {
  let isValid = true
  
  // 重置错误信息
  errors.value = {
    username: '',
    email: '',
    password: ''
  }
  
  // 校验用户名
  if (!formData.value.username.trim()) {
    errors.value.username = '用户名不能为空'
    isValid = false
  }
  
  // 校验邮箱
  if (!formData.value.email.trim()) {
    errors.value.email = '邮箱不能为空'
    isValid = false
  } else if (!emailRegex.test(formData.value.email)) {
    errors.value.email = '邮箱格式不正确'
    isValid = false
  }
  
  // 校验密码
  if (!formData.value.password) {
    errors.value.password = '密码不能为空'
    isValid = false
  } else if (formData.value.password.length < 6) {
    errors.value.password = '密码长度不少于6位'
    isValid = false
  }
  
  return isValid
}

// 处理表单提交
function handleSubmit() {
  if (validateForm()) {
    isSubmitted.value = true
    // 在实际应用中，这里会发送表单数据到服务器
    console.log('表单提交成功', formData.value)
    
    // 模拟提交成功后重置表单（可选）
    // resetForm()
  }
}

// 重置表单
function resetForm() {
  formData.value = {
    username: '',
    email: '',
    password: ''
  }
  errors.value = {
    username: '',
    email: '',
    password: ''
  }
  isSubmitted.value = false
}

// 输入框失焦时校验单个字段
function validateField(fieldName) {
  // 只在校验失败后重新校验
  if (errors.value[fieldName]) {
    validateForm()
  }
}
</script>

<template>
  <div class="form-container">
    <div class="form-header">
      <h1>用户注册表单</h1>
      <p>请填写以下信息进行注册</p>
    </div>
    
    <!-- 提交成功提示 -->
    <div v-if="isSubmitted" class="success-message">
      <p>表单提交成功！感谢您的注册。</p>
      <button @click="resetForm">重新填写</button>
    </div>
    
    <!-- 表单内容 -->
    <form v-else @submit.prevent="handleSubmit" class="registration-form">
      <!-- 用户名 -->
      <div class="form-group">
        <label for="username">用户名</label>
        <input
          id="username"
          type="text"
          v-model="formData.username"
          @blur="validateField('username')"
          @input="errors.username = ''"
          class="form-input"
          placeholder="请输入用户名"
        >
        <div v-if="errors.username" class="error-message">{{ errors.username }}</div>
      </div>
      
      <!-- 邮箱 -->
      <div class="form-group">
        <label for="email">邮箱</label>
        <input
          id="email"
          type="email"
          v-model="formData.email"
          @blur="validateField('email')"
          @input="errors.email = ''"
          class="form-input"
          placeholder="请输入邮箱"
        >
        <div v-if="errors.email" class="error-message">{{ errors.email }}</div>
      </div>
      
      <!-- 密码 -->
      <div class="form-group">
        <label for="password">密码</label>
        <input
          id="password"
          type="password"
          v-model="formData.password"
          @blur="validateField('password')"
          @input="errors.password = ''"
          class="form-input"
          placeholder="请输入密码"
        >
        <div v-if="errors.password" class="error-message">{{ errors.password }}</div>
      </div>
      
      <!-- 按钮组 -->
      <div class="form-actions">
        <button type="button" @click="resetForm" class="reset-button">重置</button>
        <button type="submit" class="submit-button">提交</button>
      </div>
    </form>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>