<template>
  <header>
    <img src="../assets/logo.svg" alt="mova logo" class="logo" />

    <button v-if="showBackButton" @click="$emit('go-back')">
      <img src="../assets/go_back.svg" alt="voltar" class="icon" />
      <span>Voltar</span>
    </button>
  </header>
</template>

<script>
export default {
  name: "AppHeader",

  props: {
    showBackButton: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 50px;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}

button {
  padding: 7px 25px 7px 10px;
  border: 1px solid var(--main-color);
  background-color: transparent;
  font-size: 14px;
  color: var(--main-color);
  transition: all 250ms;
}

button:hover {
  color: white;
  background-color: var(--main-color);
}

button:hover img {
  filter: brightness(0) saturate(100%) invert(99%) sepia(1%) saturate(4%)
    hue-rotate(332deg) brightness(102%) contrast(103%);
}

.logo {
  width: 90px;
  height: 60px;
}

.icon {
  width: 20px;
  height: 10px;
}

span {
  padding-left: 20px;
}

@media (max-width: 375px) {
  header {
    padding: 15px 20px;
  }

  button {
    border: none;
    padding: 7px 10px;
  }

  span {
    display: none;
  }
}
</style>
