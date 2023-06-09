<script setup>
import { computed } from "vue";
const props = defineProps({
  guitar: {
    type: Object,
    require: true,
  },
  cart: {
    type: Object,
    require: true,
  },
});

defineEmits([
  "addto-cart",
  "increase-amount",
  "decrease-amount",
  "delete-product",
  "empty-cart",
]);

const total = computed(() => {
  return props.cart.reduce(
    (total, product) => total + product.precio * product.amount,
    0
  );
});
</script>

<template>
  <div class="container-xl">
    <div class="row justify-content-center justify-content-md-between">
      <div class="col-8 col-md-3">
        <a href="index.html">
          <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
        </a>
      </div>
      <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
        <div class="carrito">
          <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

          <div id="carrito" class="bg-white p-3">
            <p v-if="cart.length === 0" class="text-center">
              El carrito esta vacio
            </p>
            <div v-else>
              <table class="w-100 table">
                <thead>
                  <tr>
                    <th>Imagen</th>
                    <th>Nombre</th>
                    <th>Precio</th>
                    <th>Cantidad</th>
                    <th></th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="product in cart" :key="product.id">
                    <td>
                      <img
                        class="img-fluid"
                        :src="'/img/' + product.imagen + '.jpg'"
                        :alt="product.name"
                      />
                    </td>
                    <td>{{ product.name }}</td>
                    <td class="fw-bold">${{ product.precio }}</td>
                    <td class="flex align-items-start gap-4">
                      <button
                        v-on:click="$emit('decrease-amount', product.id)"
                        type="button"
                        class="btn btn-dark"
                      >
                        -
                      </button>
                      {{ product.amount }}
                      <button
                        v-on:click="$emit('increase-amount', product.id)"
                        type="button"
                        class="btn btn-dark"
                      >
                        +
                      </button>
                    </td>
                    <td>
                      <button
                        v-on:click="$emit('delete-product', product.id)"
                        class="btn btn-danger"
                        type="button"
                      >
                        X
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>

              <p class="text-end">
                Total pagar: <span class="fw-bold">${{ total }}</span>
              </p>
              <button
                v-on:click="$emit('empty-cart')"
                class="btn btn-dark w-100 mt-3 p-2"
              >
                Vaciar Carrito
              </button>
            </div>
          </div>
        </div>
      </nav>
    </div>
    <!--.row-->

    <div class="row mt-5">
      <div class="col-md-6 text-center text-md-start pt-5">
        <h1 class="display-2 fw-bold">Modelo VAI</h1>
        <p class="mt-5 fs-5 text-white">
          {{ guitar.descripcion }}
        </p>
        <p class="text-primary fs-1 fw-black">${{ guitar.precio }}</p>
        <button
          v-on:click="$emit('addto-cart', guitar)"
          type="button"
          class="btn fs-4 bg-primary text-white py-2 px-5"
        >
          Agregar al Carrito
        </button>
      </div>
    </div>
  </div>

  <img
    class="header-guitarra"
    src="/img/header_guitarra.png"
    alt="imagen header"
  />
</template>
