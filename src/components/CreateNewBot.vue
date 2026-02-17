<template>
<div className="dark-background-overlay">
<!-- @click="setNewBotWindowStatus(false)" to close, now not working -->
<div className="centering-wrapper">
    <div className="create-new-bot-window">
        <!-- Stage 0 - bot create form; Stage 1 - waiting and granting a token -->
        <div v-if="this.stage==0">
            <h2 className="create-new-bot-text">Make a new bot</h2>
            <input v-model="botName" className="input-field" placeholder="Specify a name for the bot" maxlength=32>
            <input v-model="botDescription" className="input-field" placeholder="Specify the bot description" maxlength=100>
            <button className="input-field button" @click="createNewBot">Create</button>
        </div>
        <div v-else-if="this.stage===1 && this.botToken === ''">
            <h2 className="overlay-window-text">Requesting a bot token</h2>
            <p className="overlay-window-text">This may take a couple of seconds...</p>
        </div>
        <div v-else>
            <h2 className="create-new-bot-text">Congratulations!</h2>
            <p className="create-new-bot-text">Your bot created successfully! Here is it token. Don't show it to anyone, because it used in API to get full access to your bot:</p>
            <div className="newline">
                <p className="create-new-bot-text mono" @click="copyBotToken">{{ this.botToken }}</p>
            </div>
            <button className="input-field button" @click="exitBotCreate">Copy and leave</button>
        </div>
    </div>
</div>
</div>
</template>

<script>
import axios from 'axios';
export default {
    data() {
        return {
            botName: "",
            botDescription: "",
            stage: 0,
            botToken: ""
        }
    },
    props: {
        serverAddress: {
            type: String,
            required: true
        },
        closeNewBotWindow: {
            type: Function,
            required: true
        }
    },
    methods: {
        createNewBot() {
            if (this.botName === "" || this.botDescription === "") return;

            this.stage = 1;
            axios.post(`${this.serverAddress}/api/v0/user/bot`, {
                name: this.botName,
                description: this.botDescription
            })
            .then (response => {
                this.botToken = response.data.token;
            })
        },
        async exitBotCreate() {
            await this.copyBotToken();

            this.stage = 0;
            this.botToken = "";
            this.botName = "";
            this.botDescription = "";
            this.closeNewBotWindow();
        },
        async copyBotToken() {
            await navigator.clipboard.writeText(this.botToken);
        }
    }
}
</script>

<style scoped>
.create-new-bot-window {
    width: max-content;
    height: max-content;
    padding: 2vh 2vw 2vh 2vw;
    border-radius: 25px;
    background-color: #282b30;
    text-align: center;
}

.newline {
    display: block;
}

.mono {
    display: inline-block;
    padding: 5px 15px 5px 15px;
    border-radius: 7px;
    background-color: #1e2124;
    cursor: pointer;
    font-family: "Source Code Pro", sans-serif;
}
</style>