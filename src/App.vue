<script setup>
import { ref } from 'vue'
import axios from 'axios'

const city = ref('') // เก็บชื่อเมือง
const weather = ref(null) // เก็บข้อมูลอากาศ
const errorMessage = ref('')

const API_KEY = import.meta.env.VITE_APP_API_KEY; // 🔹 ใส่ API Key ของคุณที่นี่
console.log('API_KEY:', API_KEY)

const fetchWeather = async () => {
  if (!city.value) return
  try {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${API_KEY}&units=metric`,
    )
    weather.value = response.data
    errorMessage.value = ''
  } catch (error) {
    weather.value = null
    errorMessage.value = 'ไม่พบเมืองที่คุณค้นหา'
  }
}
</script>

<template>
  <div class="min-h-screen flex flex-col items-center justify-center bg-blue-100">
    <h1 class="text-2xl font-bold mb-4">🌤️ Weather App</h1>
    <input v-model="city" type="text" placeholder="ใส่ชื่อเมือง..." class="p-2 border rounded-md" />
    <button @click="fetchWeather" class="mt-2 bg-blue-500 text-white p-2 rounded-md">ค้นหา</button>

    <div v-if="weather" class="mt-4 bg-white p-4 rounded-md shadow-md">
      <h2 class="text-lg font-semibold">{{ weather.name }}, {{ weather.sys.country }}</h2>
      <p>🌡️ อุณหภูมิ: {{ weather.main.temp }}°C</p>
      <p>💧 ความชื้น: {{ weather.main.humidity }}%</p>
      <p>🌥️ สภาพอากาศ: {{ weather.weather[0].description }}</p>
    </div>

    <p v-if="errorMessage" class="text-red-500 mt-2">{{ errorMessage }}</p>
  </div>
</template>

<!-- ใช้ import.meta.env.VITE_APP_API_KEY แทน process.env.VUE_APP_API_KEY ในโปรเจกต์ Vue ที่ใช้ Vite.
ตั้งค่าตัวแปรใน .env ให้ถูกต้องด้วยการใช้ VITE_ นำหน้าตัวแปร. -->