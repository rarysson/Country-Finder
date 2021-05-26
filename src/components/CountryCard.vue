<template>
  <div class="country-card">
    <img :src="country.flag" alt="country flag" />
  </div>
</template>

<script>
export default {
  name: "CountryCard",

  props: {
    country: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
.country-card {
  height: 180px;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}

img {
  width: 100%;
  height: 180px;
  object-fit: cover;
}
</style>
