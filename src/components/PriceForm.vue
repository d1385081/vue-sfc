<script setup lang="ts">
import { reactive, ref } from 'vue'

const emit = defineEmits(['item-added'])

const formData = reactive({
  date: '',
  product_name: '',
  price: 0
})

const message = ref('')
const isError = ref(false)

const submitForm = async () => {
  const params = new URLSearchParams({
    date: formData.date,
    product_name: formData.product_name,
    price: formData.price.toString()
  })

  try {
    const response = await fetch(`/api/insert?${params.toString()}`)
    const data = await response.json()

    if (response.ok) {
      message.value = `✅ ${data.message}`
      isError.value = false
      emit('item-added')
      // 重設表單
      formData.date = ''
      formData.product_name = ''
      formData.price = 0
    } else {
      message.value = `❌ 錯誤: ${data.message || data.error}`
      isError.value = true
    }
  } catch (err: any) {
    message.value = `⚠️ 發生錯誤: ${err.message}`
    isError.value = true
  }
}
</script>

<template>
  <div class="form-container">
    <h2>📥 新增價格資料</h2>
    <form @submit.prevent="submitForm">
      <div>
        <label for="date">日期 (年分):</label>
        <input v-model="formData.date" type="text" id="date" placeholder="例如: 2024" required>
      </div>
      <div>
        <label for="product_name">商品名稱:</label>
        <input v-model="formData.product_name" type="text" id="product_name" placeholder="例如: 600ml可樂" required>
      </div>
      <div>
        <label for="price">商品價格 (NT$):</label>
        <input v-model.number="formData.price" type="number" id="price" step="0.1" required>
      </div>
      <button type="submit">確認新增</button>
    </form>
    <p v-if="message" :class="{ 'error-msg': isError, 'success-msg': !isError }">{{ message }}</p>
  </div>
</template>

<style scoped>
.form-container {
  margin-bottom: 20px;
}
form div {
  margin-bottom: 10px;
}
label {
  display: block;
  margin-bottom: 5px;
}
.success-msg {
  color: blue;
}
.error-msg {
  color: red;
}
input {
  padding: 5px;
  width: 200px;
}
button {
  padding: 5px 15px;
  cursor: pointer;
}
</style>
