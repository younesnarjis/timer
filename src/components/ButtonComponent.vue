<template>
    <button :class="buttonClass">
        {{ text }}
    </button>
</template>

<script>
export default {
    name: "ButtonComponent",
    props: {
        text: {
            type: String,
            default: ""
        }
    },
    computed: {
        buttonClass() {
            return this.text === "Entrar"
                ?
                    "green"
                :
                    this.text === "Salir"
                        ?
                            "pink"
                        :
                            "gray";
        }
    }
};
</script>

<style>
    button {
        display: inline-block;
        border: none;
        padding: 0.5rem 1rem;
        border-radius: 0.5rem;
        cursor: pointer;
        font-size: 16px;
        font-weight: 500;
        color: #ffffff;
        margin-left: 5px;
    }

    button.green {
        background-color: #48bb78;
    }

    button.pink {
        background-color: #f84c4c;
    }

    button.gray {
        background-color: #9b9696;
    }
</style>
