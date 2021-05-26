<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap");

/* Reset CSS */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: none;
}

/* Estilos globais */
html {
  font-family: "Montserrat", sans-serif;
}

:root {
  --main-color: #6d2080;
  --gray-color: #8d8d8d;
}

button {
  font-family: inherit;
  cursor: pointer;
  border: none;
}

input {
  font-family: inherit;
}

/* Estilos do app */
#app {
  height: 100vh;
}
</style>
