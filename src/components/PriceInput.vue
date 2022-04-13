<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
const props = defineProps<{ modelValue: number | string }>()
const emit = defineEmits(['update:modelValue', 'focus', 'blur'])

const price = ref(props.modelValue)
const tempPrice = ref<number | string>(props.modelValue)
const priceInputBlurred = ref(true)

onMounted(() => {
    handleFormatting()
})
watch(price, (val) => {
    // should only emit update when the input is in focus
    if (!priceInputBlurred.value) {
        emit('update:modelValue', val)
    }
})

watch(() => props.modelValue, (val) => {
    price.value = val
    tempPrice.value = val
})

const handleFocus = () => {
    priceInputBlurred.value = false
    emit('focus')
    if (tempPrice.value !== "") {
        price.value = tempPrice.value
    }
}
const handleBlur = () => {
    priceInputBlurred.value = true
    if (price.value.toString().trim() === "") {
        price.value = '0.0'
    }
    emit('blur')
    handleFormatting()
}
const handleFormatting = () => {
    if (price.value.toString().trim() === "") {
        tempPrice.value = price.value
        price.value = ''
    } else {
         if(Number(price.value) < 0){
            price.value = Number(price.value) * -1
        }
        tempPrice.value = price.value
        price.value = parseFloat(price.value.toString()).toFixed(2)
    }
}
</script>

<template>
    <div class="tooltip-wrapper">
        <input type="number" v-model="price" @focus="handleFocus" @blur="handleBlur" :step="0.001" />
        <div v-if="tempPrice !== '' && priceInputBlurred" class="tooltip">
            <p class="tooltip-text">{{ tempPrice }}</p>
        </div>
    </div>
</template>

<style lang="scss" scoped>
input {
    text-align: right;
    border: 2px solid black;
    height: 34px;
    width: 100%;
}

.tooltip-wrapper {
    position: relative;

    .tooltip {
        position: absolute;
        top: -24px;
        right: 0;
        background-color: white;
        box-shadow: 0px 0px 5px 0px black;
        border-radius: 5px;
        padding: 2px 5px;
        font-size: 12px;
        min-width: 50px;
        text-align: right;
        display: none;
    }

    &:hover .tooltip {
        display: block;
    }
}
</style>