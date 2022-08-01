<template>
  <div class="relative h-full min-h-screen start-container">
    <TheNavBar />
    <div class="pt-10 px-10 flex justify-center align-middle main-logo ">
      <img :width="50" :height="50" src="./assets/piggybankanim.gif" />
      <p class="text-4xl pt-4 px-2 mt-1 nes-text is-warning ">PIGGYBANK</p>
      
      <img :width="50" :height="50" src="./assets/piggybankanim.gif" />
      
    </div>

    <div class="p-10 main-container">
      
      <router-view />
    </div>

    <div class="pt-10"></div>
    <TheCat />
    <TheFooter />
  </div>
</template>

<script>
import TheNavBar from '@/components/TheNavBar';
import TheFooter from '@/components/gem-farm/TheFooter';
import TheCat from '@/components/gem-farm/TheCat';
export default {
  components: { TheFooter },
};
</script>
.
<style>
* {
  font-family: 'Press Start 2P', monospace;
}
input[type='radio']:checked + span {
  @apply text-black;
}
.main-container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100vw !important;
}

</style>
