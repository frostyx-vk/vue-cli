<template>
    <div class="message">
        <p>{{ message }}</p>
    </div>
</template>

<script>
export default {
    props: {
        message: {
            type: String,
            required: true, 
        }
    }
}
</script>

<style scoped>
    p {
        color: red;
        text-align: center;
    }
</style>