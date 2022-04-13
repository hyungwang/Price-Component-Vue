<script setup lang="ts">
import { computed, ref } from "@vue/reactivity";
import LabelInput from "./LabelInput.vue";
import PriceInput from "./PriceInput.vue";
import GhostPriceItem from "./GhostPriceItem.vue";
const currency = "EUR";

const form = ref([{ label: "Baseprice", price: 1, },]);

const totalPrice = computed(() =>
    Number(form.value.reduce((acc, cur) => acc + Number(cur.price), 0)).toFixed(2)
);
const addNewPrice = (item: { label: string, price: string }) => {
    form.value.push({
        label: item.label,
        price: Number(item.price),
    });
};
const removeItem = (index: number) => {
    form.value.splice(index, 1);
}
</script>

<template>
    <section>
        <fieldset class="price">
            <legend>Price components</legend>
            <h5 class="price__total">
                Total: &nbsp;&nbsp;
                <span class="price__total--small">{{ totalPrice }} &nbsp;{{ currency }}/KG</span>
            </h5>
            <section class="input-section">
                <section class="price-item" v-for="(item, index) in form">
                    <LabelInput v-model="item.label" :editable="index !== 0" />
                    <div class="price-input">
                        <PriceInput v-model="item.price" />
                        <button @click="removeItem(index)">
                            <img src="./../assets/remove-icon.svg" :class="['action-icon', index === 0 && 'hidden']" />
                        </button>
                    </div>

                </section>
                <GhostPriceItem @addNewItem="addNewPrice($event)" />
            </section>
        </fieldset>
    </section>
</template>

<style lang="scss" >
.price {
    display: flex;
    flex-direction: column;
    width: 650px;
    min-height: 300px;
    padding: 10px 25px 40px;
    border: 4px solid black;

    &__total {
        font-size: 1.4rem;
        text-align: right;
        padding: 0 20px;
        font-weight: 600;

        &--small {
            font-size: 1.1rem;
        }
    }
}

legend {
    font-size: 24px;
    font-weight: 500;
}

.input-section {
    margin-top: 15px;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.price-item {
    display: grid;
    grid-template-columns: 1fr 1fr;
    align-items: center;
    width: 100%;

    .price-input {
        display: grid;
        grid-template-columns: 1fr 1.5rem;
    }

    .label-input {}


    .action-icon {
        visibility: hidden;
        height: 1.25rem;
        width: 1.25rem;
        margin: 0 5px;
        cursor: pointer;

        &.hidden {
            display: none;
        }

    }

    &:hover {
        .action-icon {
            visibility: visible;
        }
    }
}
</style>
