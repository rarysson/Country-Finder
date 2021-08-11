<template>
  <div>
    <transition-group
      tag="div"
      class="countries-container"
      name="disappear-on-left"
      mode="out-in"
    >
      <country-card
        v-for="country in paginatedCountries"
        :key="country.name"
        :country="country"
        @country-selected="$emit('country-selected', $event)"
      />
    </transition-group>

    <transition name="disappear-on-down" mode="out-in">
      <pagination
        v-if="pages > 1"
        class="pagination"
        :current-page="currentPage"
        :pages="pages"
        @change-page="changePage"
      />
    </transition>
  </div>
</template>

<script>
import CountryCard from "./CountriesGrid/CountryCard.vue";
import Pagination from "./CountriesGrid/Pagination.vue";

export default {
  name: "CountriesGrid",

  props: {
    countries: {
      type: Array,
      required: true
    }
  },

  components: {
    Pagination,
    CountryCard
  },

  data() {
    return {
      paginatedCountries: [],
      currentPage: 1,
      pages: 1,
      maxCountriesPerPage: 12
    };
  },

  watch: {
    countries() {
      this.currentPage = 1;
      this.pages =
        Math.round(this.countries.length / this.maxCountriesPerPage) || 1;
      this.paginate();
    }
  },

  methods: {
    paginate() {
      const begin = (this.currentPage - 1) * this.maxCountriesPerPage;
      const end = begin + this.maxCountriesPerPage;
      this.paginatedCountries = this.countries.slice(begin, end);
    },

    changePage(page) {
      this.currentPage = page;
      this.paginate();
    }
  }
};
</script>

<style scoped>
.countries-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(275px, 1fr));
  justify-content: center;
  gap: 20px 90px;
}

.pagination {
  width: max-content;
  margin: 50px auto 0;
}
</style>
