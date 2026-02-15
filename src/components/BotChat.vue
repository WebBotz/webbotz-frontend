<template>
    <div className="container">
        <header>
            <img src="/go-back.png" className="go-back">
            <h1 className="bot-name">{{ this.bot.name }}</h1>
            <p className="bot-description">{{ this.bot.description }}</p>
        </header>
        <main>
            <div className="message-box" ref="messageBox">
                <Message v-for="message in messages" :key="message.id" :message="message" :bot="this.bot"></Message>
                <div ref="bottom-anchor"></div>
            </div>
            <div class="message-input">
                <input v-model="this.messageInput" placeholder="Write your message here..." maxlength="2048"></input>
                <img className="button" src="/send_button.png" @click="this.sendMessage"></img>
            </div>
        </main>
        
    </div>
</template>

<script>
import Message from "./Message.vue";
import axios from "axios";
import { ref, nextTick } from 'vue';

export default {
    components: { Message },
    props: {
        server_address: {
            type: String,
            required: true
        },
        bot: {
            type: Object,
            required: true
        }
    },
    data() {
        return {
            messages: [
                {
                    id: 1,
                    content: "/start",
                    by_bot: false
                },
                {
                    id: 2,
                    content: "Привет, я тестовый бот. Я могу отвечать на твои сообщения, но пока что не умею этого делать xd",
                    by_bot: true
                },
                {
                    id: 3,
                    content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam lectus. Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra nec consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget libero egestas mattis sit amet vitae augue.",
                    by_bot: true
                },
                {
                    id: 4,
                    content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam lectus. Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra nec consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget libero egestas mattis sit amet vitae augue.",
                    by_bot: false
                }
            ],
            messageInput: ""
        }
    },
    methods: {
        getNewMessages() {
            var lastReceivedId;
            if (this.messages.length == 0) {
                lastReceivedId = 0;
            } else {
                lastReceivedId = this.messages[this.messages.length - 1].id;
            }

            axios.get(`${this.server_address}/api/v0/user/messages/${this.bot.id}/${lastReceivedId}`)
                .then(response => {
                    if (response.data.length == 0) return;
                    this.messages = this.messages.concat(response.data);
                    this.scroll();
                })
        },
        sendMessage() {
            if (this.messageInput == "") return;

            axios.post(`${this.server_address}/api/v0/user/message`, {
                bot_id: this.bot.id,
                content: this.messageInput
            });
            this.messageInput = "";
        },
        async scroll() {
            await nextTick();
            const container = this.$refs.messageBox;
            if (container) {
                container.scrollTop = container.scrollHeight;
            }
        }
    },
    mounted() {
        setInterval(() => { this.getNewMessages() }, 1000);
    }
}
</script>

<style scoped>
.container {
    height: 100vh;
    display: grid;
    grid-template-rows: auto 1fr;
    box-sizing: border-box;
    overflow: hidden;
}

header {
    grid-row: 1;
    background-color:#36393e;
    padding: 1.5vh 2vw 1.5vh 2vw;
}

main {
    display: grid;
    grid-template-rows: 1fr auto;
    gap: 30px;
    min-height: 0;
    padding: 30px;
}

.go-back {
    display: inline-block;
    width: 3vh;
    margin-right: 0.75vw;

    cursor: pointer;
}

.bot-name {
    font-family: "Inter", sans-serif;
    font-weight: 700;
    text-align: left;
    display: inline-block;
    margin-right: 1vw;
}

.bot-description {
    font-family: "Inter", sans-serif;
    font-weight: 500;
    display: inline-block;
}

.message-box {
    display: block;
    overflow-y: scroll;
    grid-template-columns: 1fr 7vh;
    max-width: 100%;
}

.message-input {
    display: grid;
    height: 4vh;
    background-color: #23272a;
    padding: 1.5vh 2vw 1.5vh 2vw;

    border-radius: 25px;
    grid-template-columns: 1fr auto;
    gap: 1vw;

    box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.3);
}

input {
    display: inline-block;
    width: 100%;
    height: 100%;
    border: none;
    background-color: transparent;
    color: white;
    font-family: "Inter", sans-serif;
    font-weight: 500;
}

input:focus {
    outline: none;
}

.button {
    width: 4vh;
    height: 4vh;
    cursor: pointer;
}
</style>