<template>
  <div class="filter">
    <p class="mb-1"><strong>Бренд</strong></p>
    <v-card class="mb-3 pa-2">
      <v-checkbox
        v-for="(field, idx) in fields.brand.sort()"
        :key="idx"
        v-model="brand"
        color="primary"
        :label="field"
        :value="field"
        density="compact"
        hide-details
      >
      </v-checkbox>
    </v-card>

    <p class="mb-1"><strong>Тип</strong></p>
    <v-card class="mb-3 pa-2">
      <v-radio-group v-model="type" hide-details density="compact">
        <v-radio label="Все" value="all" color="primary"></v-radio>
        <v-radio
          v-for="(field, idx) in fields.type"
          :key="idx"
          :label="field"
          :value="field"
          class="text-capitalize"
          color="primary"
        ></v-radio>
      </v-radio-group>
    </v-card>

    <p class="mb-1"><strong>Цвет</strong></p>
    <v-card class="mb-3 pa-2">
      <v-checkbox
        v-for="(field, idx) in fields.color"
        :key="idx"
        :label="field"
        :value="field"
        class="text-capitalize"
        v-model="color"
        color="primary"
        density="compact"
        hide-details
      >
      </v-checkbox>
    </v-card>

    <p class="mb-1"><strong>Назначение</strong></p>
    <v-card class="mb-3 pa-2">
      <v-checkbox
        v-for="(field, idx) in fields.designed"
        :key="idx"
        :label="field"
        :value="field"
        class="text-capitalize"
        v-model="designed"
        color="primary"
        density="compact"
        hide-details
      >
      </v-checkbox>
    </v-card>

    <p class="mb-1"><strong>Количество отзывов (от)</strong></p>
    <v-card class="mb-3">
      <v-text-field
        v-model="reviewsAmount"
        hide-details
        single-line
        type="number"
        min="0"
        :max="fields.maxReviewsAmount"
        variant="outlined"
        density="compact"
        color="primary"
        @update:model-value="onChangeReviewsAmount"
      ></v-text-field>
    </v-card>

    <p class="mb-1"><strong>Цена</strong></p>
    <v-card class="mb-3">
      <div class="py-3 px-5">
        <v-range-slider
          v-model="range"
          :max="fields.maxPrice"
          :min="0"
          :step="1"
          thumb-label="always"
          color="primary"
          hide-details
          class="align-center pt-7 d-flex flex-column"
          @end="onChangeRange"
        >
          <template v-slot:append>
            <div class="d-flex justify-space-between align-center w-100 mt-4">
              <v-text-field
                v-model="rangeMin"
                hide-details
                single-line
                type="number"
                :max="fields.maxPrice"
                min="0"
                variant="outlined"
                density="compact"
                color="primary"
                style="width: 70px"
                @update:focused="onFocusMin"
                @update:model-value="onChangeMin"
              ></v-text-field>
              <div class="px-5">-</div>
              <v-text-field
                v-model="rangeMax"
                hide-details
                single-line
                type="number"
                :max="fields.maxPrice"
                min="0"
                variant="outlined"
                style="width: 70px"
                density="compact"
                color="primary"
                @update:focused="onFocusMax"
                @update:model-value="onChangeMax"
              ></v-text-field>
            </div>
          </template>
        </v-range-slider>
      </div>
    </v-card>
  </div>
</template>
<script setup lang="ts">
import { watch } from "vue";
import type { IFilterFields } from "@/types/IFilterFields";
import type { IFilterValues } from "@/types/IFilterValues";

const props = defineProps<{
  modelValue: IFilterValues;
  fields: IFilterFields;
}>();
const emit = defineEmits<{
  (e: "update:modelValue", payload: IFilterValues): void;
}>();

const brand = ref([]);
const color = ref([]);
const designed = ref([]);
const type = ref("all");
const reviewsAmount = ref(0);
const rangeMin = ref(0);
const rangeMax = ref(props.fields.maxPrice);
const range = ref([0, props.fields.maxPrice]);

function onChangeReviewsAmount(value: string) {
  if (reviewsAmount.value < 0) reviewsAmount.value = 0;

  reviewsAmount.value = +reviewsAmount.value;

  emit("update:modelValue", {
    brand: brand.value,
    color: color.value,
    type: type.value,
    designed: designed.value,
    reviewsAmount: reviewsAmount.value,
    price: range.value,
  });
}

function onChangeRange(e: any) {
  rangeMin.value = range.value[0];
  rangeMax.value = range.value[1];

  emit("update:modelValue", {
    brand: brand.value,
    color: color.value,
    type: type.value,
    designed: designed.value,
    reviewsAmount: reviewsAmount.value,
    price: range.value,
  });
}

function onFocusMin(e: any) {
  if (!e) {
    if (rangeMin.value > props.fields.maxPrice)
      rangeMin.value = props.fields.maxPrice;

    if (rangeMin.value < 0) rangeMin.value = 0;

    if (rangeMin.value > rangeMax.value) {
      const min = rangeMax.value;
      const max = rangeMin.value;

      rangeMin.value = min;
      rangeMax.value = max;
    }

    range.value[0] = rangeMin.value;
    range.value[1] = rangeMax.value;

    emit("update:modelValue", {
      brand: brand.value,
      color: color.value,
      type: type.value,
      designed: designed.value,
      reviewsAmount: reviewsAmount.value,
      price: range.value,
    });
  }
}
function onFocusMax(e: any) {
  if (!e) {
    if (rangeMax.value > props.fields.maxPrice)
      rangeMax.value = props.fields.maxPrice;

    if (rangeMax.value < 0) rangeMax.value = props.fields.maxPrice;

    if (rangeMax.value < rangeMin.value) {
      const min = rangeMax.value;
      const max = rangeMin.value;

      rangeMin.value = min;
      rangeMax.value = max;
    }

    range.value[0] = rangeMin.value;
    range.value[1] = rangeMax.value;

    emit("update:modelValue", {
      brand: brand.value,
      color: color.value,
      type: type.value,
      designed: designed.value,
      reviewsAmount: reviewsAmount.value,
      price: range.value,
    });
  }
}
function onChangeMin(value: string) {
  rangeMin.value = +rangeMin.value;
}
function onChangeMax(value: string) {
  rangeMax.value = +rangeMax.value;
}

watch([brand, color, type, designed], (values) => {
  emit("update:modelValue", {
    brand: values[0],
    color: values[1],
    type: values[2],
    designed: values[3],
    reviewsAmount: reviewsAmount.value,
    price: range.value,
  });
});
</script>

<style lang="scss">
.filter {
  .v-input__control {
    width: 100%;
  }
  .v-input__append {
    width: 100%;
    margin: 0;
  }
}
</style>
