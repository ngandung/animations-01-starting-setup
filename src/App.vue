<template>
    <router-view v-slot="{ Component }">
        <transition name="route" mode="out-in">
            <component :is="Component" />
        </transition>
    </router-view>
</template>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /* transition: transform 0.3s ease-out; */
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.route-enter-from {}

.router-enter-active {
    animation: slide-fade 1s ease-out;
}

.route-enter-to{}

.route-leave-active {
    animation: slide-fade 1s ease-in;
}

@keyframes slide-fade {
  0% {
    opacity: 0;
  }

  70% {
    opacity: 0.7;
  }

  100% {
    opacity: 1;
  }
}
</style>