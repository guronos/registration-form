<template>
    <div>
        <input
            class="input"
            :type="type"
            :placeholder="placeholder"
        >
    </div>
</template>
<script>
export default {
    name: "VInput",
    props: {
        type: {
            default: 'text'
        },
        placeholder: {
            default: 'Введите текст'
        }
    }
}
</script>
<style scoped>
.input {
    height: 2rem;
    padding-left: 6px;
    border-radius: 8px;
}
</style>