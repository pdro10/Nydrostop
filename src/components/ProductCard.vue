<template>
<div class="product-card" style="border: 1px solid #ccc; padding: 1rem; margin: 10px;">
    <h3>{{ product.name }}</h3>
    <p>Preço: R$ {{ product.price.toFixed(2) }}</p>
    <p>Categoria: {{ product.category.title }}</p>
    <button @click="addItem">Adicionar</button>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Product } from '@/model/product.model';
import { type PropType } from 'vue';

export default defineComponent({
    props: {
        product: {
        type: Object as PropType<Product>,
        required: true
        }
    },
    methods: {
        addItem() {
            this.$emit('add', this.product);
        }
    }
})
</script>