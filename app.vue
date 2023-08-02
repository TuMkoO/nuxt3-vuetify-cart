<template>
  <v-app id="inspire" :theme="useCartStore().getTheme">
    <Header />
    <v-main>
      <v-container>
        <NuxtLayout> <RouterView /> </NuxtLayout>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
import { useCartStore } from "./stores/cart";

useHead({
  // title: "My App",
  titleTemplate: "%s - Site Title",
  meta: [{ name: "description", content: "My amazing site." }],
  bodyAttrs: {
    class: "test",
  },
});
</script>
