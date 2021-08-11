<template>
  <div class="page-container">
    <app-header />

    <main>
      <div class="filters-container">
        <filter-select
          class="filter-select"
          :title="filterTypes.filterTitle"
          :placeholder-option="filterTypes.filterPlaceholderOption"
          :options="filterTypes.filterOptions"
          :initialValue="currentFilter"
          @filter="handleFilterChange"
        />

        <transition name="disappear-on-down" mode="out-in">
          <filter-select
            v-show="currentFilter === 'region'"
            class="secondary-filter"
            :title="filterByRegion.filterTitle"
            :placeholder-option="filterByRegion.filterPlaceholderOption"
            :options="filterByRegion.filterOptions"
            :initialValue="currentRegion"
            @filter="currentRegion = $event"
          />
        </transition>

        <transition name="disappear-on-down" mode="out-in">
          <search-input
            v-show="currentFilter !== 'region' && currentFilter"
            class="secondary-filter"
            @input="currentSearch = $event"
          />
        </transition>

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
      this.currentRegion = region.toLowerCase();
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
  border: 1px solid var(--main-color);
  border-radius: 10px;
  padding: 10px 35px;
  transition: all 250ms;
}

.search-btn:disabled {
  cursor: not-allowed;
  opacity: 75%;
}

.search-btn:hover:not(:disabled) {
  color: var(--main-color);
  background-color: white;
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
