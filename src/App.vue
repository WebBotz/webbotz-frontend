<template>
  <BotChat :bot="this.activeBot"></BotChat>
</template>

<style scoped>
</style>

<script>
import BotChat from './components/BotChat.vue';
export default {
  components: { BotChat },
  data() {
    return {
      activeBot: {
        id: 1,
        name: "Test-овый бот с длинным названием",
        description: "Это описание бота. Тут расказано о его фукнкционале"
      }
    }
  }
}
</script>
