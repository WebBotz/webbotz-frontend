<template>
    <header>
        <h1>WebBotz</h1>
        <img src="/add_bot.png" className="new-bot-button" @click="this.newBotWindowStatus=true">
    </header>
    <main>
        <h3 v-if="this.botList.length == 0" className="no-bots">It looks like you don't have any bots yet!</h3>
        <BotListRow v-for="bot in this.botList" :key="bot.id" :bot="bot" @click="setActiveBot(bot)"></BotListRow>
    </main>
    <CreateNewBot v-if="this.newBotWindowStatus==true" :closeNewBotWindow="this.closeNewBotWindow" :serverAddress="this.serverAddress"></CreateNewBot>
</template>

<script>
import CreateNewBot from './CreateNewBot.vue';
import BotListRow from './BotListRow.vue';
import axios from 'axios';

    export default {
        components: { BotListRow, CreateNewBot },
        props: {
            serverAddress: {
                type: String,
                required: true
            },
            setActiveBot: {
                type: Function,
                required: true
            },
            authToken: {
                type: String,
                required: true
            }
        },
        data() {
            return {
                botList: [],
                newBotWindowStatus: false
            }
        },
        methods: {
            updateBotList() {
                axios.get(`${this.serverAddress}/api/v0/user/bot-list`, { headers: { "Authorization": `Bearer ${this.authToken}` } })
                .then(response => {
                    this.botList = response.data
                });
            },
            closeNewBotWindow() {
                this.newBotWindowStatus = false;
                this.updateBotList();
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
    background-color:#36393e;
    padding: 1.5vh 2vw 1.5vh 2vw;

    box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.25);

    display: grid;
    grid-template-columns: 1fr 40px;
}

h1 {
    font-family: "Inter", sans-serif;
    display: inline-block;
}

.new-bot-button {
    width: 40px;
    display: inline-block;
    cursor: pointer;

    transition: 0.375s ease-out;
}

.new-bot-button:hover {
    transform: scale(1.05);
}

main {
    overflow-y: scroll;
}

.no-bots {
    font-family: "Inter", sans-serif;
    margin: 2vw;
}
</style>