<template>
  <v-app>
    <v-main>
      <profile />
      <!-- <router-view /> -->
    </v-main>
  </v-app>
</template>

<script setup>
import { RouterView } from "vue-router";
import Profile from "./components/Profile.vue";
</script>
