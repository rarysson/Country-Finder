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
          v-for="country in paginatedCountries"
          :key="country.name"
          :country="country"
          @country-selected="goToCountry"
        />
      </div>

      <pagination
        v-if="pages > 1"
        class="pagination"
        :current-page="currentPage"
        :pages="pages"
        @change-page="changePage"
      />
    </main>
  </div>
</template>

<script>
import CountryCard from "../components/CountryCard.vue";
import FilterByRegionSelect from "../components/FilterByRegionSelect.vue";
import FilterSelect from "../components/FilterSelect.vue";
import MHeader from "../components/MHeader.vue";
import Pagination from "../components/Pagination.vue";
import SearchInput from "../components/SearchInput.vue";

export default {
  name: "Home",

  components: {
    MHeader,
    FilterSelect,
    FilterByRegionSelect,
    SearchInput,
    CountryCard,
    Pagination
  },

  data() {
    return {
      currentFilter: "",
      currentRegion: "",
      currentSearch: "",
      countries: [],
      paginatedCountries: [],
      currentPage: 1,
      pages: 1,
      maxCountriesPerPage: 12
    };
  },

  computed: {
    searchDisabled() {
      return !this.currentRegion && !this.currentSearch;
    }
  },

  beforeMount() {
    const region = this.$route.params.region;

    if (region) {
      this.currentFilter = "region";
      this.currentRegion = region;
      this.searchCountry();
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
      this.currentPage = 1;
      this.pages =
        Math.round(this.countries.length / this.maxCountriesPerPage) || 1;
      this.paginate();
    },

    paginate() {
      const begin = (this.currentPage - 1) * this.maxCountriesPerPage;
      const end = begin + this.maxCountriesPerPage;
      this.paginatedCountries = this.countries.slice(begin, end);
    },

    changePage(page) {
      this.currentPage = page;
      this.paginate();
    },

    goToCountry(country) {
      this.$router.push({ name: "Country", params: { country } });
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

.pagination {
  width: max-content;
  margin: 50px auto 0;
}
</style>
