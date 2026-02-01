<template>
  <v-app>
    <v-main>
      <size-menu></size-menu>
      <!-- <profile /> -->
      <!-- <router-view /> -->
    </v-main>
  </v-app>
</template>

<script setup>
import { RouterView } from "vue-router";
import Profile from "./components/Profile.vue";
import SizeMenu from "./components/SizeMenu.vue";
</script>
