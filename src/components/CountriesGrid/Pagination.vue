<template>
  <nav>
    <button class="control-btn" @click="previousPage">&#706;</button>
    <ul>
      <li v-for="page in pages" :key="`page-${page}`">
        <button
          class="option-btn"
          :class="{ active: page === currentPage }"
          @click="selectPage(page)"
        >
          {{ page }}
        </button>
      </li>
    </ul>
    <button class="control-btn" @click="nextPage">&#707;</button>
  </nav>
</template>

<script>
export default {
  name: "Pagination",

  props: {
    pages: {
      type: Number,
      required: true
    },

    currentPage: {
      type: Number,
      required: true
    }
  },

  methods: {
    previousPage() {
      if (this.currentPage > 1) {
        this.$emit("change-page", this.currentPage - 1);
      }
    },

    nextPage() {
      if (this.currentPage < this.pages) {
        this.$emit("change-page", this.currentPage + 1);
      }
    },

    selectPage(page) {
      this.$emit("change-page", page);
    }
  }
};
</script>

<style scoped>
nav {
  display: flex;
  align-items: center;
}

ul {
  display: grid;
  gap: 10px;
  grid-auto-flow: column;
  margin: 0 15px;
}

button {
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.25);
  border: 1px solid transparent;
  border-radius: 5px;
  background-color: white;
  transition: all 250ms;
}

button:hover {
  border-color: var(--main-color);
}

.control-btn {
  width: 28px;
  height: 28px;
  color: var(--gray-color);
}

.option-btn {
  width: 35px;
  height: 35px;
  color: var(--gray-color);
}

.option-btn.active {
  color: white;
  background-color: var(--main-color);
}
</style>
