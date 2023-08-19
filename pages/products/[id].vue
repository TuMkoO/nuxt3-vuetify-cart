<template>
  <div>
    <v-row>
      <v-col>
        <h1 class="text-h2 text-capitalize mb-1">{{ product.name }}</h1>
      </v-col>
    </v-row>
    <v-card class="px-3 py-5">
      <v-row>
        <v-col cols="6">
          <v-img :src="product.image" height="400px" cover>
            <template v-slot:placeholder>
              <v-row align="center" class="fill-height ma-0" justify="center">
                <v-progress-circular
                  color="grey lighten-5"
                  indeterminate
                ></v-progress-circular>
              </v-row>
            </template>
          </v-img>
        </v-col>
        <v-col cols="6">
          <div class="d-flex justify-space-between">
            <div class="mb-4">
              <div class="mb-1">
                Тип:
                <span
                  class="text-subtitle-1 font-weight-medium text-medium-emphasis text-capitalize"
                  >{{ product.type }}</span
                >
              </div>
              <div>
                Цвет:
                <span
                  class="text-subtitle-1 font-weight-medium text-medium-emphasis text-capitalize"
                  >{{ product.color }}</span
                >
              </div>
              <div>
                Назначение:
                <v-chip
                  v-for="(design, i) in product.designed"
                  :key="i"
                  class="ma-1 text-capitalize"
                >
                  {{ design }}
                </v-chip>
              </div>
            </div>
            <h2 class="text-h6 ml-5">{{ product.brand }}</h2>
          </div>
          <v-card>
            <div class="d-flex pa-3 justify-center">
              <div class="text-h4 font-weight-bold text-primary mr-5">
                ${{ product.price }}
              </div>
              <div>
                <v-btn class="bg-primary" @click="cartStore.add(product.id)"
                  >Add to cart</v-btn
                >
              </div>
            </div>
          </v-card>
          <div class="mt-6">
            <p class="text-body-1">{{ product.description }}</p>
          </div>
          <div class="mt-6">
            Отзывы:
            <span
              class="text-subtitle-1 font-weight-medium text-medium-emphasis"
              >{{ product.reviews_amount }}</span
            >
          </div>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script setup lang="ts">
import data from "../../data";
import { useCartStore } from "../../stores/cart";

const route = useRoute();
const cartStore = useCartStore();
const products = ref(data);
const product = computed(() => {
  return products.value.filter((product) => {
    return product.id === route.params.id;
  })[0];
});

useHead({
  title:
    product.value.name.charAt(0).toUpperCase() + product.value.name.slice(1),
  meta: [
    { name: "description", content: product.value.description },
    { property: "og:type", content: "article" },
    {
      property: "og:title",
      content:
        product.value.name.charAt(0).toUpperCase() +
        product.value.name.slice(1),
    },
    { property: "og:description", content: product.value.description },
    { property: "og:url", content: `https://...` },
  ],
});
</script>
