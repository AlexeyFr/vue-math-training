<template>
    <div class="alert alert-secondary">
        <h3>Are you ready?</h3>
        <button
            class="btn btn-success"
            @click="$emit('onStart')">
            Start
        </button>
    </div>
</template>

<style scoped>
    .alert {
        text-align: center;
        background: #eeeeee;
    }
    h3, .btn {
        margin: 30px 0;
    }
</style>