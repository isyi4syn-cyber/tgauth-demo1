<template>
  <div class="container">
    <h1>🔐 Telegram Auth Demo</h1>
    <p>Тестирование аутентификации через Telegram</p>
    
    <div id="telegram-login"></div>

    <div v-if="user" class="user-info">
      <h3>✅ Вы авторизованы!</h3>
      <pre>{{ JSON.stringify(user, null, 2) }}</pre>
      <button @click="logout" class="logout-btn">Выйти</button>
    </div>

    <div v-else class="instructions">
      <p>Нажмите на кнопку выше для авторизации</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const user = ref(null)

function onTelegramAuth(userData) {
  user.value = userData
  console.log("User data:", userData)
}

function logout() {
  user.value = null
}

onMounted(() => {
  const script = document.createElement('script')
  script.async = true
  script.src = 'https://telegram.org/js/telegram-widget.js?22'
  script.setAttribute('data-telegram-login', 'YOUR_BOT_USERNAME')
  script.setAttribute('data-size', 'large') 
  script.setAttribute('data-auth-url', 'https://YOUR_SITE.netlify.app/auth')
  script.setAttribute('data-request-access', 'write')
  
  document.getElementById('telegram-login').appendChild(script)
  window.onTelegramAuth = onTelegramAuth
})
</script>

<style>
.container {
  padding: 50px;
  text-align: center;
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
}

.user-info {
  margin-top: 20px;
  padding: 20px;
  background: #f5f5f5;
  border-radius: 10px;
}

.logout-btn {
  padding: 10px 20px;
  background: #ff4444;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.instructions {
  margin-top: 20px;
  color: #666;
}
</style>