<template>
    <div className="container">
        <header>
            <img src="/go-back.png" className="go-back">
            <h1 className="bot-name">{{ this.bot.name }}</h1>
            <p className="bot-description">{{ this.bot.description }}</p>
        </header>
        <main>
            <div className="message-box">
                <Message v-for="message in messages" :key="message.id" :message="message" :bot="this.bot"></Message>
            </div>
            <div class="message-input">
                <input placeholder="Write your message here..." maxlength="2048"></input>
                <img className="button" src="/send_button.png" @click="addMessage"></img>
            </div>
        </main>
        
    </div>
</template>

<script>
import Message from "./Message.vue";

export default {
    components: { Message },
    props: {
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
                    text: "/start",
                    by_bot: false
                },
                {
                    id: 2,
                    text: "Привет, я тестовый бот. Я могу отвечать на твои сообщения, но пока что не умею этого делать xd",
                    by_bot: true
                },
                {
                    id: 3,
                    text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam lectus. Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra nec consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget libero egestas mattis sit amet vitae augue.",
                    by_bot: true
                },
                {
                    id: 4,
                    text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec a diam lectus. Sed sit amet ipsum mauris. Maecenas congue ligula ac quam viverra nec consectetur ante hendrerit. Donec et mollis dolor. Praesent et diam eget libero egestas mattis sit amet vitae augue.",
                    by_bot: false
                }
            ]
        }
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