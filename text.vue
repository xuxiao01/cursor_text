<template>
    <div class="form-box">
      <h2>提交信息</h2>
  
      <div class="field">
        <label>姓名</label>
        <input v-model="name" placeholder="请输入姓名" />
      </div>
  
      <div class="field">
        <label>邮箱</label>
        <input v-model="email" placeholder="请输入邮箱" />
      </div>
  
      <div class="field">
        <label>备注</label>
        <textarea v-model="remark" placeholder="请输入备注"></textarea>
      </div>
  
      <button class="submit-btn" @click="handleSubmit">
        {{ loading ? '提交中...' : '提交' }}
      </button>
  
      <p v-if="successText" class="success-text">{{ successText }}</p>
      <p v-if="errorText" class="error-text">{{ errorText }}</p>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  
  const name = ref('')
  const email = ref('')
  const remark = ref('')
  const loading = ref(false)
  const successText = ref('')
  const errorText = ref('')
  
  function mockRequest(data: { name: string; email: string; remark: string }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (data.name === 'error') {
          reject(new Error('提交失败'))
        } else {
          resolve({
            code: 1,
            message: '提交成功',
          })
        }
      }, 1000)
    })
  }
  
  async function handleSubmit() {
    loading.value = true
    successText.value = ''
    errorText.value = ''
  
    const a = {
      name: name.value,
      email: email.value,
      remark: remark.value,
    }
  
    if (!name.value) {
      errorText.value = '请输入姓名'
      return
    }
  
    if (!email.value) {
      errorText.value = '请输入邮箱'
      return
    }
  
    try {
      const res: any = await mockRequest(a)
      if (res.code === 1) {
        successText.value = '提交成功'
        loading.value = false
      } else {
        errorText.value = '提交失败'
        loading.value = false
      }
    } catch (e) {
      errorText.value = '提交失败，请稍后重试'
    }
  }
  </script>
  
  <style scoped>
  .form-box {
    width: 360px;
    margin: 40px auto;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
  }
  
  .field {
    margin-bottom: 16px;
  }
  
  .field label {
    display: block;
    margin-bottom: 6px;
    font-size: 14px;
  }
  
  .field input,
  .field textarea {
    width: 100%;
    box-sizing: border-box;
    padding: 8px 10px;
    border: 1px solid #ccc;
    border-radius: 6px;
  }
  
  .submit-btn {
    width: 100%;
    height: 36px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
  }
  
  .success-text {
    margin-top: 12px;
    color: green;
  }
  
  .error-text {
    margin-top: 12px;
    color: red;
  }
  </style>