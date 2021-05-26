<template>
  <header>
    <img src="../assets/logo.svg" alt="mova logo" class="logo" />

    <button @click="$emit('go-back')">
      <img src="../assets/go_back.svg" alt="voltar" class="icon" />
      <span>Voltar</span>
    </button>
  </header>
</template>

<script>
export default {
  name: "MHeader"
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
</style>
