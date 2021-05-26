<template>
  <div class="container">
    <span>Filtrar por</span>
    <div class="select-wrapper">
      <select v-model="value" @change="$emit('filter', value)">
        <option value="" disabled>Escolha uma opção</option>
        <option value="region">Região</option>
        <option value="capital">Capital</option>
        <option value="lang">Língua</option>
        <option value="name">País</option>
        <option value="callingcode">Código de ligação</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "FilterSelect",

  data() {
    return {
      value: ""
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

.select-wrapper {
  position: relative;
}

.select-wrapper::after {
  content: "\25BC";
  position: absolute;
  top: 50%;
  right: 0;
  transform: translateY(-50%);
  color: var(--gray-color);
}

select {
  border: none;
  border-bottom: 1px solid var(--gray-color);
  padding: 5px 25px 5px 5px;
  appearance: none;
  width: 100%;
  min-width: 300px;
}

option {
  font-weight: 700;
  color: var(--gray-color);
}
</style>
