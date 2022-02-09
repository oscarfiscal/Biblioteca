<!-- @format -->

<template>
  <v-app>
    <v-app-bar app color="black" dark>
      <div class="d-flex align-center">
      <v-toolbar-title>Library</v-toolbar-title>
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
