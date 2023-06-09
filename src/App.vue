<script setup>
import { db } from "./data/guitars";
import { ref, onMounted, watch } from "vue";

import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitars = ref([]);
const cart = ref([]);
const guitar = ref({});

watch(
  cart,
  () => {
    saveLocalStore();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  guitars.value = db;
  guitar.value = db[3];

  const cartStorage = localStorage.getItem("cart");
  if (cartStorage) {
    cart.value = JSON.parse(cartStorage);
  }
});

const saveLocalStore = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

const addToCart = (guitar) => {
  const existCart = cart.value.findIndex((product) => product.id === guitar.id);

  if (existCart >= 0) {
    cart.value[existCart].amount++;
  } else {
    guitar.amount = 1;
    cart.value.push(guitar);
  }
};

const decreaseAmount = (id) => {
  const index = cart.value.findIndex((product) => product.id === id);
  if (cart.value[index].amount <= 0) return;
  cart.value[index].amount--;
};
const increaseAmount = (id) => {
  const index = cart.value.findIndex((product) => product.id === id);
  if (cart.value[index].amount >= 10) return;

  cart.value[index].amount++;
};
const deleteProduct = (id) => {
  cart.value = cart.value.filter((product) => product.id !== id);
};
const emptyCart = () => {
  cart.value = [];
};
</script>

<template>
  <header class="py-5 header">
    <Header
      :guitar="guitar"
      :cart="cart"
      @addto-cart="addToCart"
      @decrease-amount="decreaseAmount"
      @increase-amount="increaseAmount"
      @delete-product="deleteProduct"
      @empty-cart="emptyCart"
    />
  </header>

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        :guitar="guitar"
        @addto-cart="addToCart"
      />
    </div>
  </main>

  <footer class="bg-dark mt-5 py-5">
    <Footer />
  </footer>
</template>
