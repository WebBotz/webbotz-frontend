<template>
  <BotChat :server_address="this.server_address" :bot="this.activeBot"></BotChat>
</template>

<style scoped>
</style>

<script>
import BotChat from './components/BotChat.vue';
export default {
  components: { BotChat },
  data() {
    return {
      server_address: "http://127.0.0.1:8000",
      activeBot: {
        id: 1,
        name: "Test-овый бот с длинным названием",
        description: "Это описание бота. Тут расказано о его фукнкционале"
      }
    }
  }
}
</script>
