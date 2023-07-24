<template>
    <div class="text-white p-4">{{ time }}</div>
</template>

<script>

export default {
    name: "TimerComponent",
    props: {
        time: {
            type: String,
            default: '00:00:00'
        }
    }
}
</script>