<template>
    <div className="message-wrapper">
        <UserMessage v-if="this.message.by_bot===false" :message="this.message"></UserMessage>
        <BotMessage v-else :message="this.message" :bot="this.bot"></BotMessage>
    </div>
</template>

<script>
import UserMessage from './message_types/UserMessage.vue';
import BotMessage from './message_types/BotMessage.vue';

    export default {
        components: { UserMessage, BotMessage },
        props: {
            message: {
                type: Object,
                required: true
            },
            bot: {
                type: Object,
                required: true
            }
        }
    }
</script>

<style scoped>
.message-wrapper {
    width: 100%;
    display: block;

    font-family: "Inter", sans-serif;
    font-weight: 500;
    font-size: 1.1em;
    margin-bottom: 1vh;

    word-break: normal;
}
</style>