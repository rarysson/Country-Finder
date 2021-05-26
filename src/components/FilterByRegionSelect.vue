<template>
  <div class="container">
    <span>Região</span>
    <div class="select-wrapper">
      <select v-model="value" @change="$emit('filter', value)">
        <option value="" disabled>Escolha uma região</option>
        <option value="africa">África</option>
        <option value="americas">Américas</option>
        <option value="asia">Ásia</option>
        <option value="europe">Europa</option>
        <option value="oceania">Oceania</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "FilterByRegionSelect",

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
