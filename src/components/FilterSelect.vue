<template>
  <div class="container">
    <span>{{ title }}</span>

    <div class="select-wrapper">
      <select v-model="value" @change="$emit('filter', value)">
        <option value="" disabled>{{ placeholderOption }}</option>

        <option
          v-for="(option, index) in options"
          :key="`option-${index}-${componentId}`"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>
    </div>
  </div>
</template>

<script>
let localId = 0;

export default {
  name: "FilterSelect",

  props: {
    title: {
      type: String,
      required: true
    },

    placeholderOption: {
      type: String,
      required: true
    },

    options: {
      type: Array,
      required: true,
      validator(value) {
        return value.every((option) => option.label && option.value);
      }
    }
  },

  data() {
    return {
      value: "",
      componentId: localId++
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
  min-width: 250px;
}

option {
  font-weight: 700;
  color: var(--gray-color);
}
</style>
