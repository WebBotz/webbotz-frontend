<template>
  <BotChat v-if="this.activeBot!=null" :serverAddress="this.serverAddress" :bot="this.activeBot" :setActiveBot="this.setActiveBot"></BotChat>
  <BotList v-else :serverAddress="this.serverAddress" :setActiveBot="this.setActiveBot"></BotList>
</template>

<script>
import BotChat from './components/BotChat.vue';
import BotList from './components/BotList.vue';

export default {
  components: { BotChat, BotList },
  data() {
    return {
      serverAddress: "http://127.0.0.1:8000",
      authToken: null,
      activeBot: null
    }
  },
  methods: {
    setActiveBot(bot) {
      this.activeBot = bot;
    }
  }
}
</script>