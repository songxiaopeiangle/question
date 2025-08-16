<script setup>
import { ref } from 'vue'

// 文件引用
const fileInput = ref(null)
// 选中的文件
const selectedFile = ref(null)
// 预览图片的URL
const previewUrl = ref('')
// 错误信息
const errorMessage = ref('')

// 处理文件选择
const handleFileChange = (event) => {
  const file = event.target.files[0]
  
  // 清空之前的错误信息
  errorMessage.value = ''
  
  if (!file) {
    selectedFile.value = null
    previewUrl.value = ''
    return
  }
  
  // 验证文件类型
  const validTypes = ['image/jpeg', 'image/png']
  if (!validTypes.includes(file.type)) {
    errorMessage.value = '只支持JPG和PNG格式的图片'
    selectedFile.value = null
    previewUrl.value = ''
    return
  }
  
  // 验证文件大小（2MB = 2 * 1024 * 1024 bytes）
  const maxSize = 2 * 1024 * 1024
  if (file.size > maxSize) {
    errorMessage.value = '文件大小不能超过2MB'
    selectedFile.value = null
    previewUrl.value = ''
    return
  }
  
  // 文件验证通过，保存文件并生成预览URL
  selectedFile.value = file
  
  // 使用FileReader生成预览URL
  const reader = new FileReader()
  reader.onload = (e) => {
    previewUrl.value = e.target.result
  }
  reader.readAsDataURL(file)
}

// 触发文件选择对话框
const triggerFileSelect = () => {
  fileInput.value?.click()
}

// 清除选中的文件
const clearFile = () => {
  selectedFile.value = null
  previewUrl.value = ''
  errorMessage.value = ''
  if (fileInput.value) {
    fileInput.value.value = ''
  }
}
</script>

<template>
  <div class="file-upload-container">
    <div class="file-upload-header">
      <h1>文件上传预览</h1>
      <p>选择JPG或PNG格式的图片（最大2MB）</p>
    </div>
    
    <div class="file-upload-content">
      <!-- 隐藏的文件输入 -->
      <input
        ref="fileInput"
        type="file"
        accept=".jpg,.jpeg,.png"
        @change="handleFileChange"
        class="hidden-file-input"
      />
      
      <!-- 图片预览区域 -->
      <div v-if="previewUrl" class="image-preview-container">
        <img :src="previewUrl" alt="预览图片" class="preview-image" />
        <div class="preview-actions">
          <p class="file-name">{{ selectedFile.name }}</p>
          <p class="file-size">{{ (selectedFile.size / 1024).toFixed(2) }} KB</p>
          <button class="remove-button" @click="clearFile">
            移除图片
          </button>
        </div>
      </div>
      
      <!-- 文件上传区域 -->
      <div v-else class="upload-area" @click="triggerFileSelect">
        <div class="upload-icon">📁</div>
        <p class="upload-text">点击或拖拽文件到此处</p>
        <p class="upload-hint">支持 JPG、PNG 格式，最大 2MB</p>
      </div>
      
      <!-- 错误信息显示 -->
      <div v-if="errorMessage" class="error-message">
        {{ errorMessage }}
      </div>
      
      <!-- 上传按钮 -->
      <div v-if="!previewUrl" class="upload-button-container">
        <button class="upload-button" @click="triggerFileSelect">
          选择文件
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>