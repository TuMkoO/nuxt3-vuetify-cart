<template>
  <v-app id="inspire" :theme="useCartStore().getTheme">
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </v-app>
</template>

<script setup lang="ts">
import { useCartStore } from "./stores/cart";

const cartStore = useCartStore();

onMounted(() => {
  const cart = localStorage.getItem("cart");

  if (cart && Object.keys(JSON.parse(cart)).length) {
    cartStore.cartContent = JSON.parse(cart);
  }

  cartStore.loader = false;
});

useHead({
  titleTemplate: "%s - Site Title",
  meta: [{ name: "description", content: "My amazing site." }],
  bodyAttrs: {
    class: "test",
  },
});
</script>
