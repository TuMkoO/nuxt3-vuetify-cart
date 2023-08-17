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
            <Filter v-model="filterValues" :fields="filterFields" />
          </div>
        </v-navigation-drawer>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4" lg="3" xl="2" class="d-none d-md-block">
        <Filter v-model="filterValues" :fields="filterFields" />
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
            v-for="(product, i) in filteredProducts"
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

              <div class="px-4 mt-2">{{ product.brand }}</div>

              <v-card-title class="text-capitalize pt-0">
                {{ product.name }}
              </v-card-title>

              <v-card-subtitle>${{ product.price }}</v-card-subtitle>

              <div
                class="px-4 d-flex justify-space-between text-subtitle-2 text-medium-emphasis text-capitalize"
              >
                <div>Color: {{ product.color }}</div>
                <div>Type: {{ product.type }}</div>
              </div>

              <div class="px-2">
                <v-chip
                  v-for="(design, i) in product.designed"
                  :key="i"
                  class="ma-1 text-capitalize"
                >
                  {{ design }}
                </v-chip>
              </div>

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
        <v-row
          v-show="!grid"
          v-for="(product, i) in filteredProducts"
          :key="i"
          class="mb-2"
        >
          <v-col cols="12" sm="6" md="4">
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
            <div class="px-4">{{ product.brand }}</div>
            <v-card-title class="text-capitalize pt-0">
              {{ product.name }}
            </v-card-title>

            <v-card-subtitle> ${{ product.price }} </v-card-subtitle>

            <div
              class="px-4 d-flex justify-end text-subtitle-2 text-medium-emphasis text-capitalize"
            >
              <div class="mx-3">Color: {{ product.color }}</div>
              <div>Type: {{ product.type }}</div>
            </div>

            <v-card-text>
              {{ product.description }}
            </v-card-text>

            <div class="px-2">
              <v-chip
                v-for="(design, i) in product.designed"
                :key="i"
                class="ma-1 text-capitalize"
              >
                {{ design }}
              </v-chip>
            </div>

            <div class="d-flex justify-end">
              <v-card-actions>
                <v-btn color="primary"> Read More </v-btn>
                <v-btn
                  class="ml-2 bg-primary"
                  @click="cartStore.add(product.id)"
                >
                  Add to Cart
                </v-btn>
              </v-card-actions>
            </div>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script setup lang="ts">
import data from "../data";
import { useCartStore } from "../stores/cart";
import type { IFilterValues } from "@/types/IFilterValues";
import type { IFilterFields } from "@/types/IFilterFields";

const cartStore = useCartStore();
const products = ref(data);
const grid = ref(true);
const drawer = ref(false);
const filterValues = ref<IFilterValues>({} as IFilterValues);
const filterFields = computed(() => {
  return {
    brand: generateUniqueField("brand"),
    color: generateUniqueField("color"),
    type: generateUniqueField("type"),
    designed: generateUniqueField("designed"),
    maxReviewsAmount: generateMaxNum("reviews_amount"),
    maxPrice: generateMaxNum("price"),
  } as IFilterFields;
});

const filteredProducts = computed(() => {
  return products.value
    .filter((product) => {
      if (filterValues.value.brand?.length) {
        return filterValues.value.brand.includes(product.brand);
      }
      return true;
    })
    .filter((product) => {
      if (filterValues.value.color?.length) {
        return filterValues.value.color.includes(product.color);
      }
      return true;
    })
    .filter((product) => {
      if (filterValues.value.type == "all") return true;
      if (filterValues.value.type) {
        return filterValues.value.type == product.type;
      }
      return true;
    })
    .filter((product) => {
      if (filterValues.value.designed?.length) {
        return product.designed.some((item) => {
          return filterValues.value.designed.includes(item);
        });
      }
      return true;
    })
    .filter((product) => {
      if (filterValues.value.reviewsAmount) {
        return filterValues.value.reviewsAmount <= product.reviews_amount;
      }
      return true;
    })
    .filter((product) => {
      if (filterValues.value.price?.length) {
        return (
          product.price >= filterValues.value.price[0] &&
          product.price <= filterValues.value.price[1]
        );
      }
      return true;
    });
});

function generateUniqueField(field: string) {
  const newArr: Array<string> = [];

  products.value.forEach((item) => {
    if (Array.isArray(item[field])) {
      newArr.push(...item[field]);
    } else {
      newArr.push(item[field]);
    }
  });

  return Array.from(new Set(newArr));
}

function generateMaxNum(field: string) {
  const newArr: Array<number> = [];

  products.value.forEach((item) => {
    newArr.push(item[field]);
  });

  return Math.max(...newArr);
}

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
