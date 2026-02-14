<template>
    <h3>You</h3><p> - {{ message.text }}</p>
</template>

<script>
export default {
    props: {
        message: {
            type: Object,
            required: true
        }
    }
}
</script>

<style scoped>
h3 {
    display: inline;
    color: rgb(204, 204, 204);
}
p {
    display: inline;
    color: rgb(204, 204, 204);
    text-align: left;
}
</style>