<template>
  <div class="py-6">
    <v-row
      v-if="!cartStore.loader"
      :class="{ 'justify-center': !cartStore.formattedCart.length }"
    >
      <v-col cols="12" :md="cartStore.formattedCart.length ? 8 : 6">
        <v-card class="py-2 py-sm-5 px-0 px-sm-4">
          <v-card-title>My Cart</v-card-title>
          <v-card-text v-if="cartStore.formattedCart.length">
            <div
              v-for="item in cartStore.formattedCart"
              :key="item.id"
              class="mb-3"
            >
              <v-row>
                <v-col cols="12" sm="4">
                  <div class="d-flex align-center no-wrap">
                    <img
                      :src="item.image"
                      alt=""
                      style="width: 90px; height: 90px; object-fit: cover"
                    />
                    <NuxtLink
                      :to="'/products/' + item.id"
                      class="text-subtitle-1 font-weight-bold text-grey-darken-3 text-decoration-none text-capitalize ml-3"
                    >
                      {{ item.name }}
                    </NuxtLink>
                  </div>
                </v-col>
                <v-col cols="12" sm="8">
                  <div
                    class="d-flex justify-space-between align-center no-wrap w-full h-100"
                  >
                    <div class="d-flex align-center bg-primary rounded">
                      <span
                        class="icon d-flex align-center pa-3"
                        style="cursor: pointer"
                        @click="addToCart(item.id)"
                      >
                        <v-icon>mdi-plus</v-icon>
                      </span>
                      <span class="number d-flex align-center pa-3">{{
                        item.quantity
                      }}</span>
                      <span
                        class="icon d-flex align-center pa-3"
                        style="cursor: pointer"
                        @click="removeFromCart(item.id)"
                      >
                        <v-icon>mdi-minus</v-icon>
                      </span>
                    </div>
                    <p class="mx-1">
                      {{ item.quantity }} X {{ item.price }} =
                      <strong>{{ item.cost }}</strong>
                    </p>
                    <v-btn @click="removeProductFromCart(item.id)" icon>
                      <v-icon color="red">mdi-delete</v-icon>
                    </v-btn>
                  </div>
                </v-col>
              </v-row>
            </div>
          </v-card-text>
          <v-card-text v-else class="text-center">
            <v-btn class="mx-auto" color="primary" @click="$router.push('/')"
              >Your cart is empty. Fill it</v-btn
            >
          </v-card-text>
        </v-card>
      </v-col>
      <v-col v-if="cartStore.total" md="4">
        <v-card class="py-2 py-sm-5 px-0 px-sm-4">
          <v-card-title class="mb-5">Order Summary</v-card-title>
          <v-card-text>
            <div class="d-flex align-center justify-space-between mb-4">
              <p class="ma-0">Subtotal</p>
              <p class="text-primary">${{ cartStore.total }}</p>
            </div>
            <div class="d-flex align-center justify-space-between mb-4">
              <p class="ma-0">Tax</p>
              <p class="text-primary">0</p>
            </div>
            <div class="d-flex align-center justify-space-between mb-4">
              <p class="ma-0">Shipping Price</p>
              <p class="text-primary">0</p>
            </div>
            <v-divider></v-divider>
            <div class="py-3 d-flex align-center justify-space-between mb-4">
              <p class="ma-0">Order Total</p>
              <p class="text-primary">${{ cartStore.total }}</p>
            </div>
            <v-btn color="primary" block disabled> Checkout </v-btn>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script setup lang="ts">
import { useCartStore } from "../stores/cart";

const cartStore = useCartStore();

function addToCart(id: string) {
  cartStore.add(id);

  localStorage.setItem("cart", JSON.stringify(cartStore.cartContent));
}

function removeFromCart(id: string) {
  cartStore.remove(id);

  localStorage.setItem("cart", JSON.stringify(cartStore.cartContent));
}

function removeProductFromCart(id: string) {
  cartStore.removeProduct(id);

  localStorage.setItem("cart", JSON.stringify(cartStore.cartContent));
}

definePageMeta({
  layout: "default",
});

useHead({
  title: "Cart",
  meta: [
    { name: "description", content: "Cart page" },
    { property: "og:type", content: "article" },
    { property: "og:title", content: `App Name Cart page` },
    { property: "og:description", content: `App Name Cart page` },
    { property: "og:url", content: `https://...` },
  ],
});
</script>
