<template>
  <div
    class="wrapper d-flex flex-column align-center justify-center"
    style="min-height: 100vh"
  >
    <div class="container text-center">
      <div class="text-h3">{{ error.statusCode }}</div>
      <div class="text-h5 mb-4">{{ error.message }}</div>
      <NuxtLink to="/" class="text-h6 text-decoration-none text-primary"
        >Go home page</NuxtLink
      >
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  error: Object,
});

useHead({
  title: props.error.statusCode + " " + props.error.message,
  meta: [
    {
      name: "description",
      content: props.error.statusCode + " " + props.error.message,
    },
    { property: "og:type", content: "article" },
    {
      property: "og:title",
      content: props.error.statusCode + " " + props.error.message,
    },
    {
      property: "og:description",
      content: props.error.statusCode + " " + props.error.message,
    },
    { property: "og:url", content: `https://...` },
  ],
});
</script>

<style lang="scss" scoped></style>
