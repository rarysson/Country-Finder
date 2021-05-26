<template>
  <div class="container">
    <span>Digite a busca do filtro</span>
    <input v-model.trim="searchTerm" @input="$emit('input', searchTerm)" />
  </div>
</template>

<script>
export default {
  name: "SearchInput",

  data() {
    return {
      searchTerm: ""
    };
  }
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}

span {
  color: var(--main-color);
  font-size: 14px;
  margin-bottom: 5px;
}

input {
  border: none;
  border-bottom: 1px solid var(--gray-color);
  padding: 5px;
  width: 100%;
  min-width: 250px;
}
</style>
