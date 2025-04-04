<template>
    <input class="input" type="text" @input="updateInput" :value="modelValue">
</template>

<script>
    export default {
        name: 'my-input',
        props: {
            modelValue: [String, Number]
        },
        methods:{
            updateInput(event){
                this.$emit('update:modelValue', event.target.value)
            }
        }
        
    }
</script>

<style scoped>
.input{
    width:100%;
    border: 1px solid rgb(63,169,126);
    padding: 0.5rem 0.75rem;
    border-radius: 6px;
    font-family: var(--font-family);
    font-feature-settings: var(--font-feature-settings, normal);
    font-size: 1rem;
}
</style>