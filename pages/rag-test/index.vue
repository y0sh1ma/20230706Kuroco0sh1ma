//http://localhost:3000/?endpoint=https://api.example.com&apiKey=abcdefg12345
//http://localhost:3000/?endpoint=https://diverta-oshima-rag.g.kuroco.app/rcms-api/3/hoken-content-search&apiKey=abc2554b2fddb788131dd63c8a9212cb4fcb7c756c458ae4e17e53ca0ee3fff7





<template>
  <div class="centered">
    <div class="layout">
      <!-- 左エリア -->
      <div class="left">
        <h2>APIリクエスト</h2>
        <form @submit.prevent="sendApiRequest">
          <label>URLエンドポイント</label>
          <input v-model="endpoint" placeholder="https://api.example.com/endpoint" />
          <label>APIキー</label>
          <input v-model="apiKey" placeholder="APIキー" />
          <label>質問事項</label>
          <textarea v-model="questionText" placeholder="質問内容を入力"></textarea>
          <button type="submit">送信</button>
        </form>
      </div>

      <!-- 右レスポンスエリア -->
      <div class="right">
        <h3>APIレスポンス</h3>

        <section v-if="responseObj && responseObj.messages" class="messages-area">
          <h4 class="section-title">結果</h4>
          <ul>
            <li v-for="(msg, i) in responseObj.messages" :key="i">{{ msg }}</li>
          </ul>
        </section>

        <section v-if="responseObj && responseObj.reply" class="reply-area">
          <h4 class="section-title">回答</h4>
          <div class="reply-content">
            {{ responseObj.reply }}
            <button class="copy-btn" @click="copyToClipboard(responseObj.reply)">コピー</button>
          </div>
        </section>

        <section v-if="responseObj" class="json-area">
          <h4 class="section-title">JSON</h4>
          <div class="json-wrapper">
            <pre>{{ responsePretty }}</pre>
            <button class="copy-btn json-copy-btn" @click="copyToClipboard(responsePretty)">コピー</button>
          </div>
        </section>

        <p v-if="!responseObj && response" class="placeholder">不明な形式のレスポンス: {{ response }}</p>
        <p v-if="!response" class="placeholder">ここにAPIの結果が表示されます。</p>

        <h3 v-if="error">エラー</h3>
        <pre v-if="error">{{ error }}</pre>
      </div>
    </div>

    <div v-if="copySuccess" class="copy-notify">コピーしました！</div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const endpoint = ref('')
const apiKey = ref('')
const questionText = ref('')
const response = ref(null)
const error = ref(null)
const copySuccess = ref(false)

onMounted(() => {
  if (route.query.endpoint) endpoint.value = route.query.endpoint
  if (route.query.apiKey) apiKey.value = route.query.apiKey
})

const responseObj = computed(() => {
  if (!response.value) return null
  try {
    const obj = typeof response.value === 'string' ? JSON.parse(response.value) : response.value
    return obj && typeof obj === 'object' ? obj : null
  } catch {
    return null
  }
})

const responsePretty = computed(() => {
  return responseObj.value ? JSON.stringify(responseObj.value, null, 2) : ''
})

const copyToClipboard = async (text) => {
  try {
    await navigator.clipboard.writeText(text)
    copySuccess.value = true
    setTimeout(() => (copySuccess.value = false), 1500)
  } catch {
    alert('コピーに失敗しました。')
  }
}

const sendApiRequest = async () => {
  response.value = null
  error.value = null
  try {
    const res = await $fetch(endpoint.value, {
      method: 'POST',
      headers: {
        'X-RCMS-API-ACCESS-TOKEN': apiKey.value,
        'Content-Type': 'application/json'
      },
      body: { text: questionText.value }
    })
    response.value = JSON.stringify(res)
  } catch (err) {
    error.value = err?.message || JSON.stringify(err)
  }
}
</script>

<style scoped>
.centered {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px 10px;
  background-color: #f9fafc;
  min-height: 100vh;
  box-sizing: border-box;
  position: relative;
}
.layout {
  display: flex;
  max-width: 1100px;
  width: 100%;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 6px 16px rgb(0 0 0 / 0.12);
  min-height: 460px;
  overflow: hidden;
}
.left {
  width: 510px;
  background: #f0f4f9;
  padding: 28px 24px;
  border-right: 1px solid #d7dfea;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  font-size: 13px;
}
.right {
  flex: 1;
  padding: 28px 24px;
  box-sizing: border-box;
  background: #fafcff;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  font-size: 13px;
}
h2,
h3 {
  margin: 0 0 20px;
  color: #2c518c;
  font-weight: bold;
  font-size: 16px;
}
form {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  gap: 16px;
}
label {
  font-weight: 600;
  font-size: 13px;
  margin-bottom: 6px;
  color: #34495e;
}
input,
textarea {
  font-size: 13px;
  padding: 7px 10px;
  border: 1px solid #b0c4de;
  border-radius: 5px;
  background: #fff;
  width: 100%;
  box-sizing: border-box;
  transition: border-color 0.2s ease;
  resize: vertical;
}
input:focus,
textarea:focus {
  border-color: #2266cc;
  outline: none;
}
textarea {
  min-height: 88px;
}
button {
  padding: 10px 0;
  background-color: #2266cc;
  border: none;
  border-radius: 5px;
  color: #fff;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}
button:hover {
  background-color: #164f9e;
}
.section-title {
  font-size: 14px;
  font-weight: 700;
  margin-bottom: 8px;
  color: #225bb5;
  border-bottom: 2px solid #c9daf8;
  padding-bottom: 5px;
}
.messages-area ul {
  padding-left: 20px;
  margin: 0 0 12px 0;
  color: #223344;
  list-style-type: disc;
}
.reply-content {
  font-size: 14px;
  padding: 12px 10px;
  background: #e9f5fa;
  color: #1a3e5b;
  border-radius: 6px;
  border: 1px solid #b3cde9;
  white-space: pre-wrap;
  word-wrap: break-word;
  min-height: 56px;
  position: relative;
}
.copy-btn {
  position: absolute;
  right: 10px;
  top: 10px;
  background-color: #3171e0;
  border: none;
  color: white;
  font-size: 12px;
  padding: 4px 10px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
.copy-btn:hover {
  background-color: #2058b4;
}
.json-area {
  position: relative;
}
.json-wrapper {
  position: relative;
}
.json-area pre {
  font-family: "Courier New", Courier, monospace;
  font-size: 12px;
  background: #f4f8fc;
  border: 1px solid #c5d1e3;
  border-radius: 6px;
  padding: 14px;
  max-height: 300px;
  overflow-y: auto;
  white-space: pre-wrap;
  word-break: break-word;
  margin: 0;
}
.json-copy-btn {
  position: absolute;
  top: 10px;
  right: 10px;
}
.placeholder {
  color: #999;
  font-style: italic;
  text-align: center;
  margin: 30px 0 0;
  font-size: 14px;
  flex-grow: 1;
}
.copy-notify {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: #3171e0;
  color: white;
  padding: 10px 20px;
  border-radius: 24px;
  font-weight: bold;
  box-shadow: 0 0 10px #3171e0aa;
  z-index: 1000;
}
</style>



