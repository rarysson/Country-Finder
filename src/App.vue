<template>
  <div id="app">
    <transition name="disappear-on-left" mode="out-in">
      <router-view />
    </transition>
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

ul {
  list-style: none;
}

/* Estilos do app */
#app {
  height: 100vh;
}

/* Transições */
.disappear-on-left-enter-active,
.disappear-on-left-leave-active {
  transition: all 250ms;
}

.disappear-on-left-enter,
.disappear-on-left-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}

.disappear-on-down-enter-active,
.disappear-on-down-leave-active {
  transition: all 250ms;
}

.disappear-on-down-enter,
.disappear-on-down-leave-to {
  opacity: 0;
  transform: translateY(-50px);
}
</style>
