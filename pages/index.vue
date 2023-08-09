<template>
  <div>
    <v-row>
      <v-col cols="12">
        <v-navigation-drawer v-model="drawer" width="400" temporary>
          <div class="px-3 py-3">
            <div class="d-flex justify-end">
              <v-btn
                icon="mdi-close"
                elevation="0"
                @click.stop="drawer = !drawer"
              >
                <v-icon> mdi-close </v-icon>
              </v-btn>
            </div>
            <Filter />
          </div>
        </v-navigation-drawer>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4" lg="3" xl="2" class="d-none d-md-block">
        <Filter />
      </v-col>
      <v-col>
        <v-row>
          <v-col cols="12">
            <div class="d-flex justify-space-between">
              <div>
                <v-btn
                  @click="grid = true"
                  :class="{ 'bg-primary': grid }"
                  class="mr-3"
                >
                  <v-icon> mdi-apps </v-icon>
                </v-btn>
                <v-btn @click="grid = false" :class="{ 'bg-primary': !grid }">
                  <v-icon> mdi-view-list </v-icon>
                </v-btn>
              </div>
              <div class="d-block d-md-none">
                <v-btn @click.stop="drawer = !drawer">
                  <v-icon> mdi-tune </v-icon>
                </v-btn>
              </div>
            </div>
          </v-col>
        </v-row>
        <v-row v-show="grid">
          <v-col
            v-for="(product, i) in products"
            :key="i"
            cols="12"
            lg="4"
            sm="6"
          >
            <v-card class="mx-auto pb-2">
              <v-img :src="product.image" height="200px" cover>
                <template v-slot:placeholder>
                  <v-row
                    align="center"
                    class="fill-height ma-0"
                    justify="center"
                  >
                    <v-progress-circular
                      color="grey lighten-5"
                      indeterminate
                    ></v-progress-circular>
                  </v-row>
                </template>
              </v-img>

              <v-card-title class="text-capitalize">
                {{ product.name }}
              </v-card-title>

              <v-card-subtitle>${{ product.price }}</v-card-subtitle>

              <v-card-actions>
                <v-btn color="primary">Read more</v-btn>
                <v-spacer></v-spacer>
                <v-btn class="bg-primary" @click="cartStore.add(product.id)"
                  >Add to cart</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
        <v-row v-show="!grid" v-for="(product, i) in products" :key="i">
          <v-col cols="4">
            <v-img :src="product.image" height="200px" cover>
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
          <v-col cols="12" md="8">
            <v-card-title class="text-capitalize">
              {{ product.name }}
            </v-card-title>

            <v-card-subtitle> ${{ product.price }} </v-card-subtitle>

            <v-card-text>
              {{ product.description }}
            </v-card-text>

            <v-card-actions>
              <v-btn color="primary"> Read More </v-btn>
              <v-btn class="ml-2 bg-primary" @click="cartStore.add(product.id)">
                Add to Cart
              </v-btn>
            </v-card-actions>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script setup lang="ts">
import data from "../data";
import { useCartStore } from "../stores/cart";

const cartStore = useCartStore();
const products = ref(data);
const grid = ref(true);

const drawer = ref(false);

watch(drawer, (newVal) => {
  if (newVal) {
    document.body.style.overflow = "hidden";
  } else {
    document.body.style.overflow = "";
  }
});

definePageMeta({
  //   title: "Shop",
  layout: "default",
});

// const route = useRoute();

useHead({
  // title: `${route.meta.title}`,
  title: "Home",
  // meta: [{ property: "og:title", content: `App Name - ${route.meta.title}` }],
  meta: [
    { name: "description", content: "Index page" },
    { property: "og:type", content: "article" },
    { property: "og:title", content: `App Name Index page` },
    { property: "og:description", content: `App Name Index page` },
    { property: "og:url", content: `https://...` },
  ],
});
</script>
