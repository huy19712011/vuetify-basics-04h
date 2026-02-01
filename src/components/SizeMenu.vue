<template>
  <VNavigationDrawer expand-on-hover permanent rail>
    <v-list>
      <v-list-item
        prepend-avatar="/vuemastery_logo.png"
        subtitle="Subtitle"
        title="Menu"
      ></v-list-item>
    </v-list>
    <VDivider></VDivider>
    <v-list density="compact" nav>
      <v-list-item
        prepend-icon="mdi-folder"
        title="My Files"
        value="myfiles"
      ></v-list-item>
      <v-list-item
        prepend-icon="mdi-account-multiple"
        title="Shared with me"
        value="shared"
      ></v-list-item>
      <v-list-item
        prepend-icon="mdi-star"
        title="Starred"
        value="starred"
      ></v-list-item>
    </v-list>
  </VNavigationDrawer>
</template>
