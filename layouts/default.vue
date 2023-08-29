<template>
  <div class="wrapper d-flex flex-column" style="min-height: 100vh">
    <Header />
    <v-main>
      <v-container>
        <slot />
      </v-container>
    </v-main>
    <Footer />
  </div>
</template>

<script setup lang="ts"></script>

<style lang="scss">
.v-main {
  padding-top: 64px !important;
}
</style>
