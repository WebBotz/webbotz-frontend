<template>
    <h3>{{ bot.name }}</h3><p> - {{ message.text }}</p>
</template>

<script>
export default {
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
h3 {
    display: inline;
}
p {
    display: inline;
    color: rgb(230, 230, 230);
    text-align: left
}
</style>