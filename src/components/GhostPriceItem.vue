
<script lang="ts" setup>
import { ref, watch } from 'vue'
import LabelInput from './LabelInput.vue'
import PriceInput from './PriceInput.vue'
const emit = defineEmits(['addNewItem'])
const label = ref('')
const price = ref<number | ''>('')

const labelInputBlurred = ref(true)
const priceInputBlurred = ref(true)

watch([label, price, labelInputBlurred, priceInputBlurred], () => {
    if (priceInputBlurred.value && labelInputBlurred.value && label.value.trim().length > 0 && Number.isFinite(price.value)) {
        emit('addNewItem', { label: label.value, price: price.value })
        label.value = '',
        price.value = ''
    }
})
</script>
<template>
    <section class="ghost-price-item">
        <LabelInput ref="labelInput" :ghostMode="true" v-model="label" editable @focus="labelInputBlurred = false"
            @blur="labelInputBlurred = true" />
        <div class="price-input">
            <PriceInput @focus="priceInputBlurred = false" @blur="priceInputBlurred = true" v-model="price" />
        </div>
    </section>

</template>



<style lang="scss" scoped>
.ghost-price-item {
    display: grid;
    grid-template-columns: 4fr 5fr;
    align-items: center;
    opacity: 0.4;

    .price-input {
        display: grid;
        grid-template-columns: 1fr 1.5rem;
        padding-left: 32px;
    }
}
</style>