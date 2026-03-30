<template>
    <div class="upload-panel">
      <h2>文件上传</h2>
  
      <input type="file" @change="handleFileChange" />
  
      <div v-if="fileName" class="file-info">
        当前文件：{{ fileName }}
      </div>
  
      <div v-if="errorText" class="error-text">
        {{ errorText }}
      </div>
  
      <div v-if="successText" class="success-text">
        {{ successText }}
      </div>
  
      <button class="upload-btn" @click="handleUpload">
        {{ loading ? '上传中...' : '开始上传' }}
      </button>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  const currentFile = ref<any>(null)
  const fileName = ref('')
  const loading = ref(false)
  const errorText = ref('')
  const successText = ref('')
  
  function mockUpload(file: File) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (file.name.includes('fail')) {
          reject(new Error('上传失败'))
        } else {
          resolve({
            code: 1,
            message: '上传成功',
          })
        }
      }, 1200)
    })
  }
  
  function handleFileChange(e: Event) {
    const target = e.target as HTMLInputElement
    const f = target.files && target.files[0]
  
    currentFile.value = f
    fileName.value = f ? f.name : ''
    errorText.value = ''
    successText.value = ''
  }
  
  async function handleUpload() {
    loading.value = true
    errorText.value = ''
    successText.value = ''
  
    if (!currentFile.value) {
      errorText.value = '请先选择文件'
      return
    }
  
    if (currentFile.value.size > 10 * 1024 * 1024) {
      errorText.value = '文件不能超过10MB'
      return
    }
  
    const a = currentFile.value
  
    try {
      const res: any = await mockUpload(a)
  
      if (res.code === 1) {
        successText.value = '上传成功'
        loading.value = false
      } else {
        errorText.value = '上传失败'
        loading.value = false
      }
    } catch (err) {
      errorText.value = '上传失败，请稍后重试'
    }
  }
  </script>
  
  <style scoped>
  .upload-panel {
    width: 400px;
    margin: 40px auto;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
  }
  
  .file-info {
    margin-top: 12px;
    font-size: 14px;
  }
  
  .upload-btn {
    margin-top: 16px;
    width: 100%;
    height: 36px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
  }
  
  .error-text {
    margin-top: 12px;
    color: red;
  }
  
  .success-text {
    margin-top: 12px;
    color: green;
  }
  </style>