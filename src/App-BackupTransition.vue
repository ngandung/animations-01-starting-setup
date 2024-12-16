<template>
  <div class="container">
    <div class="block" :class="{animate: animatedBlock}"></div>
    <button @click="animateBlock">Animate</button>
  </div>

  <div class="container">
    <user-list></user-list>
  </div>

  <div class="container">
    
    <!-- <Transition> is Vue default component to control animation -->
    <!-- Transition only habve one direct element -->
    <!-- :css is used to tell Vue that this transition is not using CSS code -->
     <transition 
      :css="false"
      @before-enter="onBeforeEnter" 
      @enter="onEnter"
      @before-leave="onBeforeLeave"
      @leave="onLeave"
      @enter-cancelled="onEnterCancelled"
      @leave-cancelled="onLeaveCancelled"
     >
      <p v-if="paraIsVisible">This is only sometimes visible...</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>
 
    <base-modal @close="hideDialog" :open="dialogIsVisible">
      <p>This is a test dialog!</p>
      <button @click="hideDialog">Close it!</button>
    </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>


  <div class="container">
      <Transition name="showing">
        <p v-if="show">hello</p>
      </Transition>
      <button @click="show = !show">Toggle</button>
  </div>
</template>  

<script>
import UserList from './components/UserList.vue';

export default {
  components: {
    UserList
  },
  data() {
    return { 
      dialogIsVisible: false,
      animatedBlock: false,
      paraIsVisible: false,
      show: false,
      enterInterval: null,
      leaveInterval: null,
     };
  },
  methods: {
    toggleParagraph() {
      this.paraIsVisible = !this.paraIsVisible;
    },
    animateBlock() {
      this.animatedBlock = true;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    onEnterCancelled() {
      clearInterval(this.enterInterval);
    },
    onLeaveCancelled() {
      clearInterval(this.leaveInterval);
    },
    onBeforeEnter(el) {
      console.log(this.onBeforeEnter);
      el.style.opacity = 0;
    }, 
    onEnter(el, done) {
      let round = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;

        if (round >= 100) {
          clearInterval(this.enterInterval);
          done();
        }
      } , 20);
    },
    onBeforeLeave(el) {
      el.style.opacity = 1;
    },
    onLeave(el, done) {
      let round = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round*0.01;
        round++;

        if(round > 100) {
          clearInterval(this.leaveInterval);
          done();
        }
      },20);
    }
  },
};
</script>

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

.animate {
  /* transform: translateX(-150px); */
  /* forwards is use so animation stay in final state and not backward */
  animation: slide-fade 1s ease-out forwards;
}

/*
.para-enter-active {
  animation: slide-fade 0.3s ease-in;
}

.para-leave-active {
  animation: slide-fade 0.3s ease-out;
}

@keyframes slide-fade {
  0% {
    transform: translateX(0) scale(1);
  }

  70% {
    transform: translateX(-120px) scale(1.1);
  }

  100% {
    transform: translateX(-150px) scale(1);
  }
}
*/

/* .v-enter-from {
  opacity: 0;
  transform: translateY(-30px);
} */

.v-enter-active {
  transition: all 1s ease-in;
}

/* .v-enter-to {
  opacity: 1;
  transform: translateY(0px);
} */

/* we will explain what these classes do next! */
.showing-enter-active,
.showing-leave-active {
  transition: opacity 0.5s ease;
}

.showing-enter-from,
.showing-leave-to {
  opacity: 0;
}




</style>