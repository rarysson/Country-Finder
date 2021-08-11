<template>
  <div lass="page-container">
    <app-header show-back-button @go-back="$router.push('/')" />

    <main v-if="country !== null">
      <img :src="country.flag" alt="country flag" />

      <ul>
        <li>Nome: {{ country.name }}</li>
        <li>Capital: {{ country.capital }}</li>
        <li>
          Região:
          <span class="link" @click="goToRegion">{{ country.region }}</span>
        </li>
        <li>Sub-região: {{ country.subregion }}</li>
        <li>População: {{ country.population }}</li>
        <li>Linguas: {{ getLangs(country.languages) }}</li>
      </ul>
    </main>

    <footer>
      <p>Países Vizinhos:</p>

      <countries-grid
        :countries="countries"
        @country-selected="changeCountry"
      />
    </footer>
  </div>
</template>

<script>
import AppHeader from "../components/AppHeader.vue";
import CountriesGrid from "../components/CountriesGrid.vue";

export default {
  name: "Country",

  components: {
    AppHeader,
    CountriesGrid
  },

  data() {
    return {
      country: null,
      countries: []
    };
  },

  beforeMount() {
    this.country = this.$route.params.country;
    this.setNeighbors(this.country.borders);
  },

  methods: {
    getLangs(languages) {
      return languages.map((lang) => lang.nativeName).join(", ");
    },

    async setNeighbors(neighbors) {
      const countries = [];

      for (let i = 0; i < neighbors.length; i++) {
        const response = await fetch(
          `https://restcountries.eu/rest/v2/alpha/${neighbors[i]}`
        );
        const country = await response.json();
        countries.push(country);
      }

      this.countries = countries;
    },

    changeCountry(country) {
      this.country = country;
      this.setNeighbors(country.borders);
    },

    goToRegion() {
      this.$router.push({
        name: "Home",
        params: { region: this.country.region }
      });
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
  padding: 150px 25px 100px;
  display: flex;
}

main img {
  width: 450px;
  height: 250px;
  object-fit: cover;
  margin-right: 25px;
}

.link {
  color: var(--main-color);
  text-decoration: underline;
  cursor: pointer;
}

ul {
  display: grid;
  gap: 25px;
  color: #454545;
}

footer {
  max-width: 1125px;
  margin: 0 auto;
  padding: 0 25px 75px;
}

footer p {
  margin-bottom: 50px;
}

@media (max-width: 768px) {
  main img {
    width: 300px;
    height: 180px;
  }
}

@media (max-width: 500px) {
  main {
    padding-top: 50px;
    flex-direction: column;
  }

  main img {
    width: 100%;
    margin-right: 0;
    margin-bottom: 25px;
  }
}
</style>
