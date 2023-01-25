<script setup>
import Navbar from './components/Navbar.vue';
</script>

<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
    <Navbar />
    <router-view></router-view>
  </div>
</template>