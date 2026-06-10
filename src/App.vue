<script setup lang="ts">
import { ref, onMounted } from 'vue'
import PriceForm from './components/PriceForm.vue'
import PriceTable from './components/PriceTable.vue'
import type { ColaItem } from './types'

const allData = ref<ColaItem[]>([
  { date: '1990 年代初期', product_name: '600ml 可樂', price: 20 },
  { date: '1998 - 2005 年', product_name: '600ml 可樂', price: 25 },
  { date: '2011 年左右', product_name: '600ml 可樂', price: 29 },
  { date: '2022 年 4 月', product_name: '600ml 可樂', price: 35 },
  { date: '2024 年 2 月', product_name: '600ml 可樂', price: 39 },
  { date: '2026 年 (現在)', product_name: '600ml 可樂', price: 42 }
])

const fetchColaPrices = async () => {
  try {
    const response = await fetch('/api/quotes')
    if (response.ok) {
      const data = await response.json()
      if (data && data.length > 0) {
        allData.value = data
      }
    } else {
      console.warn('API 無回應，載入預設資料')
    }
  } catch (err) {
    console.warn('載入 API 失敗，使用預設資料:', err)
  }
}

onMounted(() => {
  fetchColaPrices()
})

const handleItemAdded = () => {
  fetchColaPrices()
}
</script>

<template>
  <div id="app-container">
    <h1>🥤 肥宅快樂水：可樂歷年價格查詢系統</h1>
    <p>追蹤那些年我們一起喝的快樂水價格變化。</p>
    
    <hr>
    
    <PriceForm @item-added="handleItemAdded" />
    
    <hr>
    
    <PriceTable :items="allData" />
  </div>
</template>

<style>
body {
  font-family: sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #fff;
}
#app-container {
  max-width: 900px;
  margin: 0 auto;
}
hr {
  margin: 20px 0;
  border: 0;
  border-top: 1px solid #ccc;
}
h1 {
  color: #333;
}
</style>
