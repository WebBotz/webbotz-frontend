<template>
<div className="dark-background-overlay">
<div className="centering-wrapper">
    <div className="auth-box">
        <h2 className="overlay-window-text">Authorization required!</h2>
        <input v-model="this.authPassword" className="input-field" placeholder="Enter your password...">
        <p v-if="this.error != ''" className="overlay-window-text error">{{ this.error }}</p>
        <button className="input-field button" @click="auth">Auth</button>
    </div>
</div>
</div>
</template>

<script>
import axios from "axios";
import Cookies from "js-cookie";

export default {
    props: {
        serverAddress: {
            type: String,
            required: true
        },
        setAuthToken: {
            type: Function,
            required: true
        }
    },
    data() {
        return {
            authPassword: "",
            error: ""
        }
    },
    methods: {
        auth() {
            if (this.authPassword === "") return;
            axios.post(`${this.serverAddress}/api/v0/auth/token`, {
                password: this.authPassword
            }).then(response => {
                this.error = "";
                Cookies.set("auth-token", response.data.token, { expires: 7 });
                this.setAuthToken(response.data.token);
            }).catch(error => {
                this.error = "Wrong password!";
            });
        }
    }
}
</script>

<style scoped>
.auth-box {
    width: max-content;
    height: max-content;
    padding: 2vh 2vw 2vh 2vw;
    border-radius: 25px;
    background-color: #282b30;
    text-align: center;
    font-family: "Inter", sans-serif;
}

.error {
    color: red;
}
</style>