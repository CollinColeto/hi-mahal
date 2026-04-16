<template>
  <div class="chat-toggle-button" @click="showChat = !showChat">💬</div>

  <transition name="slide-fade">
    <div v-show="showChat" class="floating-bot-wrapper">
      <div class="bot-chat-row">
        <div class="chat-container" ref="chatContainer">
          <div v-for="(msg, index) in messages" :key="index" class="chat-message" :class="msg.from">
            {{ msg.text }}
          </div>
        </div>

        <div class="chat-bot" :class="{ 'slide-in': entered }">
          <div class="character">
            <div class="head">
              <div class="hair"></div>
              <span class="eye left"></span>
              <span class="eye right"></span>
              <div class="glasses">
                <div class="lens left"></div>
                <div class="lens right"></div>
                <div class="bridge"></div>
              </div>
              <div class="mouth" :class="{ talking: botTalking }"></div>
            </div>
            <div class="body">
              <div class="suit"></div>
              <div class="tie"></div>
            </div>
          </div>
        </div>
      </div>

      <div class="chat-input">
        <input
          v-model="userText"
          @keyup.enter="sendMessage"
          type="text"
          placeholder="Type your message..."
        />
        <button @click="sendMessage">Send</button>
      </div>
    </div>
  </transition>
</template>

<script setup>
import { ref, nextTick, onMounted } from 'vue'

const messages = ref([])
const userText = ref('')
const entered = ref(false)
const botTalking = ref(false)
const chatContainer = ref(null)
const showChat = ref(false)

const botReplies = [
  'Hi, Mahal ❤️',
  'I hope you do not stress yourself too much',
  'I know medyo nainis ka today di lng sakin pati na rin sa mga alaga mo',
  'I hope kahit papano malighten up ko ang mood mo',
  'I love you always mahal ko',
]
]

let typing = false

function scrollToBottom() {
  nextTick(() => {
    if (chatContainer.value) {
      chatContainer.value.scrollTop = chatContainer.value.scrollHeight
    }
  })
}

async function typeBotMessage(text) {
  if (typing) return
  typing = true
  botTalking.value = true
  messages.value.push({ text: '', from: 'bot' })
  scrollToBottom()

  for (let i = 0; i < text.length; i++) {
    messages.value[messages.value.length - 1] = {
      text: text.slice(0, i + 1),
      from: 'bot',
    }
    await nextTick()
    scrollToBottom()
    await new Promise((r) => setTimeout(r, 50))
  }

  botTalking.value = false
  typing = false
}

function sendMessage() {
  if (!userText.value.trim()) return
  messages.value.push({ text: userText.value, from: 'user' })
  scrollToBottom()
  const userReply = userText.value.toLowerCase()
  userText.value = ''

  let botText = ''
  if (userReply.includes('love')) {
    botText = 'I love you more! ✨'
  } else if (userReply.includes('thanks') || userReply.includes('thank')) {
    botText = 'You’re welcome! ❤️'
  } else {
    botText = botReplies[Math.floor(Math.random() * botReplies.length)]
  }

  typeBotMessage(botText)
}

onMounted(async () => {
  setTimeout(() => (entered.value = true), 400)

  await typeBotMessage('Hi, Mahal! ❤️')
  await new Promise((r) => setTimeout(r, 400))
  await typeBotMessage('I love you always mahal!')
  await new Promise((r) => setTimeout(r, 400))
  await typeBotMessage('Mwa mwa mwa mwa mwa mwa')
})
</script>
