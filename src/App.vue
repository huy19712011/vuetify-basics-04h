<template>
  <v-app>
    <v-container>
      <!-- <v-text-field v-model="msg" /> -->
      <!-- <v-card>{{ msg }}</v-card> -->

      <v-card>
        <v-row class="ma-0 pa-0">
          <v-col cols="6" class="bg-blue">
            <v-img
              src="https://cdn.vuetifyjs.com/docs/images/brand-kit/v-logo-circle.svg"
              :width="300"
              cover
              class="mx-auto"
            >
            </v-img>
            <h1 class="text-center">Vuetify Company</h1>
          </v-col>
          <v-col
            cols="6"
            class="d-flex flex-column justify-center align-center"
          >
            <div>
              <div>
                <h2>John Leider</h2>
                <p>CEO, Vuetify Company</p>
              </div>
              <div class="d-flex pt-4">
                <v-icon class="mr-2">mdi-phone</v-icon>
                <p>0123 456 789</p>
              </div>
              <div class="d-flex pt-4">
                <v-icon class="mr-2">mdi-map-marker</v-icon>
                <p>Vuetify Land</p>
              </div>
              <div class="d-flex pt-4">
                <v-icon class="mr-2">mdi-email</v-icon>
                <p>john@vuetify.com</p>
              </div>
            </div>
          </v-col>
        </v-row>
      </v-card>
    </v-container>
  </v-app>
</template>

<script setup>
//
const msg = ref("Hello Vuetify 3");
</script>

<style scoped>
.container {
  width: 500px;
  padding: 16px;
  background-color: red;
}
</style>
