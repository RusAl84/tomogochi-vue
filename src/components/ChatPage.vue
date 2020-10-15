<template>
  <v-container>

    <v-card>
      <v-card-title>Tomagochi</v-card-title>

      <!-- Панель сообщений -->
      <v-card-text>
        <div class="messages-view">

          <div class="message" v-for="(message, i) in messages" :key="i">
            <span class="message-date mr-3">{{ new Date(message.timestamp).toDateString() }}</span>
            <span class="message-username mr-5">{{ message.username }}</span>
            <span class="message-text">{{ message.text }}</span>
          </div>

        </div>
      </v-card-text>

      <!-- Панель действий -->
      <v-card-text>
        <div>
          <v-text-field style="width: 40%; min-width: 200px" outlined dense v-model="username" label="Username"/>
        </div>
        <div class="d-flex">
          <v-text-field class="mr-10" outlined dense v-model="message" label="message"/>
          <v-btn
              depressed
              color="primary"
              @click="onSendClick"
              :disabled="!message.length"
          >
            <v-icon>mdi-send</v-icon>
          </v-btn>
        </div>
      </v-card-text>

    </v-card>

  </v-container>
</template>

<script>
import API from '../api/api.js';

export default {
  // Модель данных страницы
  data: () => {
    return {
      username: '',
      message: '',
      // messages: [
      //   {"username": "fddffdfdfd", "text": "texttexttexttexttexttext1", "timestamp": "2020-10-15T17:05:12.417897Z"},
      //   {"username": "fddffdfdfd", "text": "texttexttexttexttexttext2", "timestamp": "2020-10-15T17:05:12.417897Z"},
      //   {"username": "fddffdfdfd", "text": "texttexttexttexttexttext3", "timestamp": "2020-10-15T17:05:12.417897Z"},
      //   {"username": "fddffdfdfd", "text": "texttexttexttexttexttext4", "timestamp": "2020-10-15T17:05:12.417897Z"},
      //   {"username": "fddffdfdfd", "text": "texttexttexttexttexttext5", "timestamp": "2020-10-15T17:05:12.417897Z"},
      //   {"username": "fddffdfdfd", "text": "texttexttexttexttexttext6", "timestamp": "2020-10-15T17:05:12.417897Z"},
      // ],
      messages: [],
      intervalCtx: null,
      lastMsgID: 0,
    }
  },

  // Хук который сработает когда страница создасться
  mounted() {
    this.username = new Array(5)
        .fill('')
        .map(
            () => 'abcdefghijkel'
                .split('')[(Math.random() * 13) | 0])
        .join('')
    this.intervalCtx = setInterval(async () => {
      try {
        const msg = await API.getMessage(this.lastMsgID)
        if (typeof msg === typeof {}) {
          this.messages.push(msg)
          this.lastMsgID++
        }
      } catch (e) {
        console.error(e)
      }
    }, 100)
  },

  methods: {
    // Реакция на кнопку отправки
    async onSendClick() {
      try {
        await API.sendMessage(this.username, this.message)
        console.log('cleared')
        this.message = ''
      } catch (e) {
        console.error(e)
      }
    },
  },

  // После уничтожения компонента
  destroyed() {
    clearInterval(this.intervalCtx)
  }
}
</script>

<style lang="sass" scoped>
.messages-view
  //border: 1px solid black
  border-radius: 3px
  overflow-x: hidden
  overflow-y: scroll
  height: 40vh

  .message
    background: #9fc8ea
    border-radius: 3px
    padding: 3px
    margin: 5px

    .message-date
      color: rgba(255, 255, 255, 0.9)
    .message-username
      color: rgba(255, 255, 0, 0.8)
    .message-text
      color: rgba(0, 0, 0, 0.9)
</style>
