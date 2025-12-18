<template>
    <div class="input-field-div">
        <slot name="label"/>
        <div class="input-field" :class="hasError ? 'err-field' : ''">
            <div class="unit-measurement" :class="hasError ? 'err-measurement' : ''" v-if="isCurrency"> <b> £ </b> </div>
            <input 
                :value="modelValue" 
                @input="$emit('update:modelValue', $event.target.value)"
                type="number" 
            />
            <div class="unit-measurement" :class="hasError ? 'err-measurement' : ''" v-if="!isCurrency"> <b> <slot name="unit"/> </b> </div>
        </div>
        <div class="error-msg" v-if="hasError">
            This field is required
        </div>
    </div>
</template>

<script>
export default {
    name: "InputField",
    props: {
        isCurrency: {
            type: Boolean,
            default: false
        },
        modelValue: [String, Number],
        hasError: {
            type: Boolean,
            default: false,
        }
    },
    emits: ['update:modelValue']
};
</script>

<style scoped>
.input-field-div {
    margin: 1.5rem;
}
.input-field-div:focus-within .input-field {
    outline: 1px solid var(--primary-color)
}
.input-field-div:focus-within .unit-measurement {
    background-color: var(--primary-color)
}
.input-field {
    outline: 1px solid var(--slate-500);
    border-radius: 5px;
    display: flex;
    align-items: stretch;
    padding: 0;  
    margin-top: 0.75rem;
    &:hover {
        outline: 1px solid var(--slate-900)
    }
}
.unit-measurement {
    display: inline-block;
    padding: 1rem;
    background-color: var(--color-background);
    width: fit-content;
}
input {
    border: none;
    outline: none;
    padding: 1rem;
    font-size: 1.2rem;
    flex-grow: 1;
    font-weight: bold;
    width: 100%; 
    box-sizing: border-box;
}

.error-msg {
    color: var(--error-color);
    margin-top: 1rem;
}
.err-field {
    outline: 1px solid var(--error-color);
}
.err-measurement {
    background-color: var(--error-color);
    color: var(--white);
}
</style>