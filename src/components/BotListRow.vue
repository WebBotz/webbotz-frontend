<template>
    <div className="bot-list-row">
        <h2>{{ bot.name }}</h2>
        <p>{{ bot.description }}</p>
    </div>
</template>

<script>
    export default {
        props: {
            bot: {
                type: Object,
                required: true
            }
        }
    }
</script>

<style>
.bot-list-row {
    padding: 2vh 2vw 2vh 2vw;
    border-bottom: 1px solid #36393e;

    font-family: "Inter", sans-serif;
    cursor: pointer;

    transition: 0.5s ease-in-out;
}

.bot-list-row:hover {
    background-color: #2a2d31;
}
</style>