<!-- @format -->

<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="desserts"
          :search="search"
          :sort-by="sortBy.toLowerCase()"
          class="elevation-12"
        >
          <template v-slot:top>
            <v-toolbar class="mb-2" color="black" dark>
              <v-spacer></v-spacer>
            </v-toolbar>
            <v-col cols="12" sm="12">
              <v-text-field
                v-model="search"
                clearable
                flat
                solo-inverted
                hide-details
                prepend-inner-icon="mdi-magnify"
                label="search books"
              ></v-text-field>
            </v-col>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Biblioteca",
  data: () => ({
    search: "",
    sortBy: "name",
    headers: [
      {
        text: "Title",
        align: "start",
        sortable: false,
        value: "title",
      },

      { text: "Subtitle", value: "subtitle", sortable: false },
      { text: "isbn13", value: "isbn13", sortable: false },
      { text: "Price", value: "price", sortable: false },
      { text: "Url", value: "url", sortable: false },
    ],
    desserts: [],
  }),
};
</script>
