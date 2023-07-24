<template>
    <div class="avatar-circle">
        <img alt="employee avatar" :src="requireSrc">
        <span :class="status === 'offline' ? 'status-red' : 'status-green'" />
    </div>
</template>

<script>
export default {
    name: "AvatarComponent",
    props: {
        status: {
            type: String,
            required: true
        },
        src: {
            type: String,
            default: ""
        }
    },
    computed: {
        requireSrc() {
            return this.src ? require(this.src) : require("../assets/images/employee.png");
        }
    }
}
</script>

<style>
    .avatar-circle {
        width: 48px;
        height: 48px;
        border-radius: 50%;
        background-color: #cccccc;
        overflow: hidden;
        position: relative;
        margin-left: 5px;
    }

    .avatar-circle img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .status-red,
    .status-green {
        position: absolute;
        bottom: 28px;
        right: -1px;
        width: 9px;
        height: 9px;
        border-radius: 3px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .status-red {
        background-color: #ff0000;
    }

    .status-green {
        background-color: #ffe135;
    }
</style>
