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
          <v-btn depressed color="primary">
            <v-icon>mdi-send</v-icon>
          </v-btn>
        </div>
      </v-card-text>

    </v-card>

  </v-container>
</template>

<script>
export default {
  // Модель данных страницы
  data: () => {
    return {
      username: '',
      message: '',
      messages: [
        {"username": "fddffdfdfd", "text": "texttexttexttexttexttext", "timestamp": "2020-10-15T17:05:12.417897Z"},
        {"username": "fddffdfdfd", "text": "text", "timestamp": "2020-10-15T17:05:12.417897Z"},
        {"username": "fddffdfdfd", "text": "text", "timestamp": "2020-10-15T17:05:12.417897Z"},
        {"username": "fddffdfdfd", "text": "text", "timestamp": "2020-10-15T17:05:12.417897Z"},
        {"username": "fddffdfdfd", "text": "text", "timestamp": "2020-10-15T17:05:12.417897Z"},
        {"username": "fddffdfdfd", "text": "text", "timestamp": "2020-10-15T17:05:12.417897Z"},
      ],
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
    background: #1E88E5
    border-radius: 3px
    padding: 3px
    margin: 5px

    .message-date, .message-username
      color: rgba(255, 255, 255, 0.5)

    .message-text
      color: white
</style>
