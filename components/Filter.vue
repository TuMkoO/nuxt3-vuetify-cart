<template>
  <div class="filter">
    <p class="mb-1"><strong>Бренд</strong></p>
    <v-card class="mb-3">
      <v-checkbox
        v-model="selected"
        color="primary"
        label="John"
        value="John"
        hide-details
      >
      </v-checkbox>
      <v-checkbox
        v-model="selected"
        color="primary"
        label="Jacob"
        value="Jacob"
        hide-details
      ></v-checkbox>
    </v-card>

    <p class="mb-1"><strong>Тип</strong></p>
    <v-card class="mb-3"> 123 </v-card>

    <p class="mb-1"><strong>Цвет</strong></p>
    <v-card class="mb-3">
      <v-radio-group hide-details>
        <v-radio label="Radio 1" value="1" color="primary"></v-radio>
        <v-radio label="Radio 2" value="2" color="primary"></v-radio>
        <v-radio label="Radio 3" value="3" color="primary"></v-radio>
      </v-radio-group>
    </v-card>

    <p class="mb-1"><strong>Назначение</strong></p>
    <v-card class="mb-3"> 123 </v-card>

    <p class="mb-1"><strong>Рейтинг</strong></p>
    <v-card class="mb-3"> 123 </v-card>

    <p class="mb-1"><strong>Количество отзывов (от)</strong></p>
    <v-card class="mb-3"> 123 </v-card>

    <p class="mb-1"><strong>Цена</strong></p>
    <v-card class="mb-3">
      <div class="py-3 px-5">
        <v-range-slider
          v-model="range"
          :max="100"
          :min="0"
          :step="1"
          thumb-label="always"
          color="primary"
          hide-details
          class="align-center pt-7 d-flex flex-column"
        >
          <!-- <template v-slot:prepend>
            <v-text-field
              :model-value="range[0]"
              hide-details
              single-line
              type="number"
              variant="outlined"
              density="compact"
              style="width: 70px"
              @change="$set(range, 0, $event)"
            ></v-text-field>
          </template> -->
          <template v-slot:append>
            <div class="d-flex justify-space-between align-center w-100 mt-4">
              <v-text-field
                :model-value="range[0]"
                hide-details
                single-line
                type="number"
                variant="outlined"
                density="compact"
                color="primary"
                style="width: 70px"
                @change="$set(range, 0, $event)"
              ></v-text-field>
              <div class="px-5">-</div>
              <v-text-field
                :model-value="range[1]"
                hide-details
                single-line
                type="number"
                variant="outlined"
                style="width: 70px"
                density="compact"
                color="primary"
                @change="$set(range, 1, $event)"
              ></v-text-field>
            </div>
          </template>
        </v-range-slider>
      </div>
    </v-card>
  </div>
</template>
<script setup lang="ts">
const selected = ref([]);
const range = ref([0, 50]);
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
