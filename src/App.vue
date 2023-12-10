<script setup>
import {ref, onMounted} from 'vue'
const show = ref(false);
</script>

<template>
    <button @click='show = !show' >Toggle Message</button>
  <transition name='fade'>
    <p v-if='show'>Hello, Vue 3 Transition Component!</p>
  </transition>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>