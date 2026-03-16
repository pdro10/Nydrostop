<template>
  <main>
    <h1>Minha Loja</h1>
    <div style="display: flex; flex-wrap: wrap;">
      <ProductCard
        v-for="p in products"
        :key="p.id"
        :product="p"
        @add="addToCart" />
    </div>

    <hr />

    <section>
      <h2>Resumo do Carrinho</h2>
      <p>Total de itens: **{{ cart.totalItems }}**</p>
      <p>Preço Final: **R$ {{ cart.finalPrice.toFixed(2) }}**</p>

      <ul>
        <li v-for="item in cart.list" :key="item.product.id">
          {{ item.product.name }} x {{ item.quantity }}
        </li>
      </ul>
    </section>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Product } from './model/product.model';
import { Category } from './model/category.model';
import { Cart } from './model/cart.model';
import ProductCard from './components/ProductCard.vue';

export default defineComponent({
  components: { ProductCard },
  data() {
    return {
      products: [
        new Product(1, "Notebook", 4000, new Category(1, "Hardware")),
        new Product(2, "Mouse", 100, new Category(1, "Periféricos")),
      ] as Product[],

      cart: new Cart([], 0, 0)
    }
  },

  methods: {
    addToCart(product: Product) {
      const existItem = this.cart.list.find(item => item.product.id === product.id);

      if (existItem) {
        existItem.quantity++;
      } else {
        this.cart.list.push({ product: product, quantity: 1 });
      }

      this.updateCartTotals();
    },

    updateCartTotals() {
      let totalQtd = 0;
      let totalPrice = 0;

      this.cart.list.forEach(item => {
        totalQtd += item.quantity;
        totalPrice += (item.product.price * item.quantity);
      });

      this.cart.totalItems = totalQtd;
      this.cart.finalPrice = totalPrice;
    }
  }
})
</script>