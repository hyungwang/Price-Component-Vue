<script setup lang="ts">
import { ref, watch } from 'vue'
const props = defineProps({
    editable: {
        type: Boolean,
        required: true
    },
    modelValue: {
        type: String,
        required: true
    },
    ghostMode: {
        type: Boolean,
        default: false
    }
})
const emit = defineEmits(['update:modelValue', 'focus', 'blur'])

const label = ref(props.modelValue)
const tempLabel = ref("")
const editMode = ref(props.ghostMode)

watch(label, (val) => {
    emit('update:modelValue', val)
})
watch(() => props.modelValue, (val) => {
    label.value = val
})

const handleBlur = () => {
    if (!props.ghostMode) {
        editMode.value = false
    }
    if (label.value.trim() === "" && tempLabel.value !== "") {
        label.value = tempLabel.value
    }
    emit('blur')
}
const handleFocus = () => {
    tempLabel.value = label.value
    emit('focus')
}
</script>

<template>
    <section :title="label" class="label-section">
        <input @blur="handleBlur" @focus="handleFocus" type="text" class="label-input" v-if="editMode"
            v-model="label" />
        <p class="label-text" v-else>{{ label }}</p>
        <button v-if="props.editable" @click="editMode = true">
            <img :class="['action-icon']" src="./../assets/edit-icon.svg" />
        </button>
    </section>


</template>

<style lang="scss" scoped>
.label-section {
    font-weight: 500;
    font-size: 18px;
    display: flex;
    justify-content: space-between;
    padding-right: 25px;

    section {
        margin-right: auto;
    }

    width: 100%;
    overflow-x: hidden;
}

.label-text {
    font-weight: 500;
    font-size: 18px;
    width: 100%;
    white-space: nowrap;
    overflow-x: hidden;
    text-overflow: ellipsis;
}

.label-input {
    height: 34px;
    border: 2px solid black;
    width: 100%;
    font-weight: 500;
    font-size: 16px;

    &:focus {
        outline: none;
    }
}
</style>