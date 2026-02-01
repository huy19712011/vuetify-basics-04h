<script setup lang="ts">
import Highlights from "./Highlights.vue";
import Likes from "./Likes.vue";
import Media from "./Media.vue";
import Replies from "./Replies.vue";
import Tweet from "./Tweet.vue";

const current_tab = ref(null);
const tabList = [
  { title: "Posts", value: "posts", component: Tweet },
  { title: "Replies", value: "replies", component: Replies },
  { title: "Highlights", value: "highlights", component: Highlights },
  { title: "Media", value: "media", component: Media },
  { title: "Likes", value: "likes", component: Likes },
];
</script>

<template>
  <v-container class="w-50 pa-0">
    <v-img src="/vuemastery_header.png" height="300" cover></v-img>
    <VContainer class="pa-0 px-6">
      <VRow justify="space-between" class="profile">
        <VCol cols="auto">
          <v-avatar image="/vuemastery_logo.png" size="200"></v-avatar>
        </VCol>
        <VCol cols="auto" class="mt-6" align-self="center">
          <v-btn variant="outlined" rounded="xl">Edit Profile</v-btn>
        </VCol>
      </VRow>
      <div class="pl-8">
        <h2>Vue Mastery</h2>
        <p>@VueMastery</p>

        <p class="py-2">
          The ultimate learning platform for Vue.js developers.<br />
          Learn from the experts, <br />Elevate your code, <br />Master your
          craft
        </p>
        <a href="vuemastery.com/courses">vuemastery.com/courses</a>
      </div>

      <VRow class="pl-6 pt-6">
        <VCol class="d-flex" cols="auto">
          <VIcon icon="mdi-briefcase-outline"></VIcon>
          <span>Science & Technology</span>
        </VCol>
        <v-col class="d-flex" cols="auto">
          <v-icon icon="mdi-map-marker-outline"> </v-icon>
          <span>Orlando, FL</span>
        </v-col>

        <v-col class="d-flex" cols="auto">
          <v-icon icon="mdi-link-variant"> </v-icon>
          <a href="vuemastery.com">vuemastery.com</a>
        </v-col>

        <v-col class="d-flex" cols="auto">
          <v-icon icon="mdi-calendar-month"> </v-icon>
          <span>Joined December, 2017</span>
        </v-col>
      </VRow>

      <!--  -->
      <v-row class="pl-6">
        <v-col cols="auto">
          <h4>696 following</h4>
        </v-col>
        <v-col cols="auto">
          <h4>24k followers</h4>
        </v-col>
      </v-row>
      <!--  -->
      <VTabs class="pl-6" v-model="current_tab">
        <VTab
          v-for="tab in tabList"
          :key="tab.value"
          :value="tab.value"
          color="blue"
        >
          {{ tab.title }}
        </VTab>
      </VTabs>

      <v-window v-model="current_tab">
        <v-window-item
          v-for="tab in tabList"
          :value="tab.value"
          :key="tab.value"
        >
          <component :is="tab.component" />
        </v-window-item>
      </v-window>
    </VContainer>
  </v-container>
</template>

<style scoped>
.profile {
  margin-top: -100px;
}
</style>
