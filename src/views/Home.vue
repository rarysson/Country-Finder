<template>
  <div class="page-container">
    <m-header />

    <main>
      <div class="filters-container">
        <filter-select @filter="handleFilterChange" />
        <filter-by-region-select
          v-show="currentFilter === 'region'"
          @filter="currentRegion = $event"
        />
        <search-input
          v-show="currentFilter !== 'region' && currentFilter"
          @input="currentSearch = $event"
        />
        <button
          class="search-btn"
          :disabled="searchDisabled"
          @click="searchCountry"
        >
          Pesquisar
        </button>
      </div>

      <div class="countries-container">
        <country-card
          v-for="country in countries"
          :key="country.name"
          :country="country"
        />
      </div>
    </main>
  </div>
</template>

<script>
import CountryCard from "../components/CountryCard.vue";
import FilterByRegionSelect from "../components/FilterByRegionSelect.vue";
import FilterSelect from "../components/FilterSelect.vue";
import MHeader from "../components/MHeader.vue";
import SearchInput from "../components/SearchInput.vue";

export default {
  name: "Home",

  components: {
    MHeader,
    FilterSelect,
    FilterByRegionSelect,
    SearchInput,
    CountryCard
  },

  data() {
    return {
      currentFilter: "",
      currentRegion: "",
      currentSearch: "",
      countries: []
    };
  },

  computed: {
    searchDisabled() {
      return !this.currentRegion && !this.currentSearch;
    }
  },

  methods: {
    handleFilterChange(filter) {
      this.currentFilter = filter;
      this.currentRegion = "";
      this.currentSearch = "";
    },

    async searchCountry() {
      const url = this.currentRegion
        ? `https://restcountries.eu/rest/v2/region/${this.currentRegion}`
        : `https://restcountries.eu/rest/v2/${this.currentFilter}/${this.currentSearch}`;
      const response = await fetch(url);
      const countries = await response.json();

      this.countries = countries;
    }
  }
};
</script>

<style scoped>
.page-container {
  height: 100%;
}

main {
  max-width: 1125px;
  margin: 0 auto;
  padding: 150px 0 50px;
}

.filters-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 50px;
}

.countries-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(315px, 1fr));
  justify-content: center;
  gap: 20px 90px;
}

.search-btn {
  text-transform: uppercase;
  color: white;
  background-color: var(--main-color);
  border-radius: 10px;
  padding: 10px 35px;
}

.search-btn:disabled {
  cursor: not-allowed;
  opacity: 75%;
}
</style>
