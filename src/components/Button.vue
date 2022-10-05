<template>
    <button class="btn" :style="{background: color }" @click="onClick()">
        {{text}}
    </button>
</template>

<script lang="ts">
export default {
    name: 'Button',
    props: {
        text: String,
        color: String
    },
    methods: {
        onClick() {
            alert('Clicked')
        }
    }
}
</script>