<script setup lang="ts">
import { ref, computed } from 'vue'
import type { ColaItem } from '../types'

const props = defineProps<{
  items: ColaItem[]
}>()

const searchQuery = ref('')

const filteredData = computed(() => {
  const query = searchQuery.value.toLowerCase()
  if (!query) return props.items
  return props.items.filter(item => 
    item.date.toString().toLowerCase().includes(query) || 
    item.product_name.toLowerCase().includes(query)
  )
})
</script>

<template>
  <div>
    <div class="search-box">
      <h2>🔍 簡易查詢</h2>
      <label for="searchInput">輸入關鍵字過濾 (年分或名稱):</label><br>
      <input 
        v-model="searchQuery" 
        type="text" 
        id="searchInput" 
        placeholder="搜尋..."
        class="search-input"
      >
    </div>

    <h2>📊 價格變化一覽表</h2>
    <table border="1">
      <thead>
        <tr>
          <th>日期 (年分)</th>
          <th>商品名稱</th>
          <th>商品價格 (元)</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in filteredData" :key="index">
          <td>{{ item.date }}</td>
          <td>{{ item.product_name }}</td>
          <td>{{ item.price }} 元</td>
        </tr>
        <tr v-if="filteredData.length === 0">
          <td colspan="3" style="text-align: center;">暫無資料</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.search-box {
  margin: 20px 0;
  padding: 10px;
  background: #f4f4f4;
  border-radius: 5px;
}
.search-input {
  margin-top: 10px;
  padding: 5px;
  width: 250px;
}
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}
th, td {
  padding: 8px;
  text-align: left;
}
th {
  background-color: #eee;
}
</style>
