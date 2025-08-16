<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import Chart from 'chart.js/auto'

// 图表引用
const chartRef = ref(null)
let chartInstance = null

// 本地模拟数据 - 每月销售额
const salesData = {
  labels: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
  values: [65000, 59000, 80000, 81000, 56000, 85000, 90000, 92000, 88000, 95000, 102000, 110000]
}

// 初始化图表
const initChart = () => {
  // 获取canvas上下文
  const ctx = chartRef.value.getContext('2d')
  
  // 如果已有图表实例，先销毁
  if (chartInstance) {
    chartInstance.destroy()
  }
  
  // 创建新的图表实例
  chartInstance = new Chart(ctx, {
    type: 'bar', // 柱状图
    data: {
      labels: salesData.labels,
      datasets: [{
        label: '月销售额 (元)',
        data: salesData.values,
        backgroundColor: 'rgba(100, 108, 255, 0.7)',
        borderColor: 'rgba(100, 108, 255, 1)',
        borderWidth: 1,
        borderRadius: 6,
        barThickness: 30,
        categoryPercentage: 0.7,
        hoverBackgroundColor: 'rgba(83, 91, 242, 0.9)'
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        title: {
          display: true,
          text: '年度销售额统计',
          font: {
            size: 18,
            weight: 'bold'
          },
          color: '#fff',
          padding: {
            top: 10,
            bottom: 20
          }
        },
        legend: {
          display: true,
          position: 'top',
          labels: {
            color: '#ddd',
            font: {
              size: 12
            }
          }
        },
        tooltip: {
          backgroundColor: 'rgba(0, 0, 0, 0.8)',
          titleColor: '#fff',
          bodyColor: '#ddd',
          borderColor: 'rgba(100, 108, 255, 0.5)',
          borderWidth: 1,
          padding: 12,
          cornerRadius: 8,
          callbacks: {
            label: function(context) {
              const value = context.raw
              // 格式化数字为货币格式
              return `销售额: ¥${value.toLocaleString()}`
            }
          }
        }
      },
      scales: {
        x: {
          grid: {
            display: false,
            drawBorder: false
          },
          ticks: {
            color: '#aaa'
          }
        },
        y: {
          beginAtZero: true,
          grid: {
            color: 'rgba(255, 255, 255, 0.1)'
          },
          ticks: {
            color: '#aaa',
            callback: function(value) {
              // 格式化y轴刻度为万元
              if (value >= 10000) {
                return (value / 10000).toFixed(0) + '万'
              }
              return value
            }
          }
        }
      }
    }
  })
}

// 页面挂载后初始化图表
onMounted(() => {
  initChart()
  
  // 监听窗口大小变化，重新渲染图表
  const handleResize = () => {
    if (chartInstance) {
      chartInstance.resize()
    }
  }
  
  window.addEventListener('resize', handleResize)
  
  // 组件卸载时清理事件监听
  onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
    if (chartInstance) {
      chartInstance.destroy()
    }
  })
})

// 图表类型切换（可选功能）
const toggleChartType = (type) => {
  if (chartInstance) {
    chartInstance.destroy()
    chartInstance = null
    
    const ctx = chartRef.value.getContext('2d')
    chartInstance = new Chart(ctx, {
      type: type,
      data: chartInstance.data,
      options: chartInstance.options
    })
  }
}
</script>

<template>
  <div class="chart-container">
    <div class="chart-header">
      <h1>简单图表展示</h1>
      <p>基于Chart.js的销售数据可视化</p>
    </div>
    
    <div class="chart-content">
      <!-- 图表容器 -->
      <div class="chart-wrapper">
        <canvas ref="chartRef"></canvas>
      </div>
      
      <!-- 数据信息 -->
      <div class="data-info">
        <h3>数据概览</h3>
        <div class="data-stats">
          <div class="stat-item">
            <span class="stat-label">年度总销售额</span>
            <span class="stat-value">¥{{ (salesData.values.reduce((a, b) => a + b, 0)).toLocaleString() }}</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">最高月销售额</span>
            <span class="stat-value">¥{{ Math.max(...salesData.values).toLocaleString() }}</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">最低月销售额</span>
            <span class="stat-value">¥{{ Math.min(...salesData.values).toLocaleString() }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 样式已在 style.css 中定义 */
</style>