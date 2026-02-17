<template>
  <div v-if="this.authToken != null">
    <!-- Authorization required -->
    <BotChat v-if="this.activeBot!=null" 
    :serverAddress="this.serverAddress" 
    :bot="this.activeBot" 
    :setActiveBot="this.setActiveBot"
    :authToken="this.authToken">
    </BotChat>
    <BotList v-else 
      :serverAddress="this.serverAddress" 
      :setActiveBot="this.setActiveBot"
      :authToken="this.authToken">
    </BotList>
  </div>
  <Auth v-else 
    :serverAddress="this.serverAddress"
    :setAuthToken="this.setAuthToken">
  </Auth>
</template>

<script>
import Auth from './Auth.vue';
import BotChat from './components/BotChat.vue';
import BotList from './components/BotList.vue';
import Cookies from 'js-cookie';
import axios from 'axios';

export default {
  components: { BotChat, BotList, Auth },
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
    },
    setAuthToken(token) {
      this.authToken = token;
    }
  },
  mounted() {
    var notConfirmedauthToken = (Cookies.get("auth-token") || null)
    axios.post(`${this.serverAddress}/api/v0/auth/token-validate`, {
      token: notConfirmedauthToken
    }).then(response => {
      this.authToken = notConfirmedauthToken;
    }).catch(error => {
      console.log("Token expired");
    })
  }
}
</script>