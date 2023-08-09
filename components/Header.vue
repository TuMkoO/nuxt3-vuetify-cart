<template>
  <v-app-bar>
    <v-toolbar-title @click="$router.push('/')" style="cursor: pointer"
      >Shopping Website</v-toolbar-title
    >
    <v-spacer></v-spacer>
    <v-btn @click="$router.push('/cart')" class="text-none" stacked>
      <v-badge :content="cartStore.productsTotal" color="error">
        <v-icon>mdi-cart-outline</v-icon>
      </v-badge>
    </v-btn>
    <v-btn
      class="d-none d-md-flex"
      :prepend-icon="
        cartStore.theme === 'light' ? 'mdi-weather-sunny' : 'mdi-weather-night'
      "
      @click="cartStore.toggleTheme"
    >
      Toggle Theme
    </v-btn>
    <v-btn class="d-md-none" @click="cartStore.toggleTheme">
      <v-icon v-if="cartStore.theme === 'light'"> mdi-weather-sunny </v-icon>
      <v-icon v-else> mdi-weather-night </v-icon>
    </v-btn>
  </v-app-bar>
</template>

<script setup>
import { useCartStore } from "../stores/cart";

const cartStore = useCartStore();
</script>
