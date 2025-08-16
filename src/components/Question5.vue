<script setup>
import { ref } from 'vue'

// 控制模态框显示状态
const isModalVisible = ref(false)

// 打开模态框
const openModal = () => {
  isModalVisible.value = true
}

// 关闭模态框
const closeModal = () => {
  isModalVisible.value = false
}

// 点击背景区域关闭模态框
const handleBackdropClick = (e) => {
  if (e.target === e.currentTarget) {
    closeModal()
  }
}
</script>

<template>
  <div class="modal-demo-container">
    <div class="modal-demo-header">
      <h1>模态弹窗组件</h1>
      <p>点击下方按钮打开模态弹窗</p>
    </div>
    
    <div class="modal-demo-content">
      <button class="open-modal-button" @click="openModal">
        打开模态弹窗
      </button>
    </div>
    
    <!-- 模态弹窗组件 -->
    <div v-if="isModalVisible" class="modal-backdrop" @click="handleBackdropClick">
      <div class="modal-container">
        <div class="modal-header">
          <h2>模态弹窗标题</h2>
          <button class="modal-close-button" @click="closeModal">
            ×
          </button>
        </div>
        
        <div class="modal-body">
          <p>这是模态弹窗的自定义内容区域。</p>
          <p>您可以在这里放置任何需要展示的内容，包括表单、图片、文本等。</p>
          <div class="modal-example-content">
            <div class="example-item">
              <div class="example-icon">📝</div>
              <div class="example-text">表单填写</div>
            </div>
            <div class="example-item">
              <div class="example-icon">📊</div>
              <div class="example-text">数据展示</div>
            </div>
            <div class="example-item">
              <div class="example-icon">⚠️</div>
              <div class="example-text">重要提示</div>
            </div>
          </div>
        </div>
        
        <div class="modal-footer">
          <button class="modal-cancel-button" @click="closeModal">
            取消
          </button>
          <button class="modal-confirm-button" @click="closeModal">
            确认
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>