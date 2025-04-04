<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: 'my-button'
    }
</script>

<style scoped>
.btn{
    width:100%;
    border: 1px solid rgb(63,169,126);
    padding: 0.5rem 0.75rem;
    border-radius: 6px;
    font-family: var(--font-family);
    font-feature-settings: var(--font-feature-settings, normal);
    font-size: 1rem;
    background: rgba(63,169,126, .3);
}
</style>