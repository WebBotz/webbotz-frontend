<template>
    <header>
        <h1>WebBotz</h1>
    </header>
    <main>
        <h3 v-if="this.botList.length == 0" className="no-bots">It looks like you don't have any bots yet!</h3>
        <BotListRow v-for="bot in this.botList" :key="bot.id" :bot="bot" @click="setActiveBot(bot)"></BotListRow>
    </main>
</template>

<script>
import BotListRow from './BotListRow.vue';
import axios from 'axios';

    export default {
        components: { BotListRow },
        props: {
            serverAddress: {
                type: String,
                required: true
            },
            setActiveBot: {
                type: Function,
                required: true
            }
        },
        data() {
            return {
                botList: []
            }
        },
        methods: {
            updateBotList() {
                axios.get(`${this.serverAddress}/api/v0/user/bot-list`)
                .then(response => {
                    this.botList = response.data
                });
            }
        },
        mounted() {
            this.updateBotList();
            setInterval(() => { this.updateBotList() }, 60*1000);
        }
    }
</script>

<style scoped>
header {
    grid-row: 1;
    background-color:#36393e;
    padding: 1.5vh 2vw 1.5vh 2vw;

    box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.25);
}

main {
    overflow-y: scroll;
}

.no-bots {
    font-family: "Inter", sans-serif;
    margin: 2vw;
}
</style>