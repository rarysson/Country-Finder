<template>
  <div class="page-container">
    <app-header />

    <main>
      <div class="filters-container">
        <filter-select
          :title="filterTypes.filterTitle"
          :placeholder-option="filterTypes.filterPlaceholderOption"
          :options="filterTypes.filterOptions"
          @filter="handleFilterChange"
        />

        <filter-select
          v-show="currentFilter === 'region'"
          :title="filterByRegion.filterTitle"
          :placeholder-option="filterByRegion.filterPlaceholderOption"
          :options="filterByRegion.filterOptions"
          @filter="currentRegion = $event"
        />

        <search-input
          v-show="currentFilter !== 'region' && currentFilter"
          class="secondary-filter"
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

      <countries-grid :countries="countries" @country-selected="goToCountry" />
    </main>
  </div>
</template>

<script>
import FilterSelect from "../components/FilterSelect.vue";
import AppHeader from "../components/AppHeader.vue";
import SearchInput from "../components/SearchInput.vue";
import CountriesGrid from "../components/CountriesGrid.vue";

export default {
  name: "Home",

  components: {
    AppHeader,
    FilterSelect,
    SearchInput,
    CountriesGrid
  },

  data() {
    return {
      currentFilter: "",
      currentRegion: "",
      currentSearch: "",
      countries: [],
      filterTypes: {
        filterTitle: "Filtrar por",
        filterPlaceholderOption: "Escolha uma opção",
        filterOptions: [
          { label: "Região", value: "region" },
          { label: "Capital", value: "capital" },
          { label: "Língua", value: "lang" },
          { label: "País", value: "name" },
          { label: "Código de ligação", value: "callingcode" }
        ]
      },
      filterByRegion: {
        filterTitle: "Região",
        filterPlaceholderOption: "Escolha uma região",
        filterOptions: [
          { label: "África", value: "africa" },
          { label: "Américas", value: "americas" },
          { label: "Ásia", value: "asia" },
          { label: "Europa", value: "europe" },
          { label: "Oceania", value: "oceania" }
        ]
      }
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
  padding: 150px 25px 50px;
}

.filters-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 50px;
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

@media (max-width: 900px) {
  .filters-container {
    display: grid;
    grid-template:
      "filter secondary"
      ". btn";
    gap: 25px;
  }

  .search-btn {
    grid-area: btn;
  }
}

@media (max-width: 600px) {
  main {
    padding-top: 50px;
  }

  .filters-container {
    grid-template:
      "filter filter"
      "secondary secondary"
      ". btn";
  }

  .filter-select {
    grid-area: filter;
  }

  .secondary-filter {
    grid-area: secondary;
  }
}
</style>
