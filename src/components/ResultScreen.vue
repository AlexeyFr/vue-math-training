<template>
    <div class="alert alert-primary">
        <h3>Success: {{ stats.success }}</h3>
        <h3>Error: {{ stats.error }}</h3>
        <button
            class="btn btn-primary"
            @click="$emit('repeat')">
            Repeat
        </button>
        <button
            class="btn btn-primary"
            @click="$emit('nextLevel')"
            v-if="buttonNextLevel">
            Next Level
        </button>
        <button
            class="btn btn-primary"
            @click="$emit('start')"
            v-else>
            Start
        </button>
    </div>
</template>

<script>
    export default {
        props: ['stats', 'buttonNextLevel'],
    }
</script>

<style scoped>
    .alert {
        text-align: center;
        background: #eeeeee;
    }
    h2, .btn {
        margin: 20px 0;
    }
</style>