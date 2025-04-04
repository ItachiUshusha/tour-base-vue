<template>
    <select 
        required 
        class="select-items" 
        @change="changeOption" 
        :value="modelValue" 
        >
        <option 
            selected hidden
            value=""
            >{{ selectText }}
        </option>
        <option 
            v-for="option in options"
            :key="option.value"
            :value="option.name"
            >
            {{ option.name }}
        </option>
    </select>
</template>

<script>
    export default {
        name: 'my-select',
        props: {
            modelValue:{
                type: String
            },
            options:{
                type: Array,
                default: () => []
            },
            selectText:{
                type: String
            }
        },
        methods: {
            changeOption(event) {
                this.$emit('update:modelValue', event.target.value)
            }
        }
    }
</script>

<style scoped>
.select-items{
    width:100%;
    border: 1px solid rgb(63,169,126);
    padding: 0.5rem 0.75rem;
    border-radius: 6px;
    font-family: var(--font-family);
    font-feature-settings: var(--font-feature-settings, normal);
    font-size: 1rem;
    appearance: none;
}
</style>