<!-- @format -->

<template>
  <div>
    <h1 class="title">consult</h1>
    <!-- //Input para consultar libro -->
    <v-text-field
      v-model="search"
      clearable
      flat
      solo-inverted
      hide-details
      prepend-inner-icon="mdi-magnify"
      label="search books"
      @keyup.enter="searchData"
    ></v-text-field>
    <v-btn depressed @click="searchData" color="primary"> Consultar </v-btn>

    <!--    //tabla para listar los libros -->
    <v-card elevation="4" class="pa-2" outlined tile>
      <v-data-table
        :headers="headers"
        :items="desserts"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.title="{ item }">
          <v-chip @click="modal" white>
            {{ item.title }}
          </v-chip>
        </template>
        
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Biblioteca",
  data: () => ({
    page: 1,
    pages: 1,
    search: "",
    url: "",
    sortBy: "name",
    headers: [
      {
        text: "Title",
        align: "start",
        sortable: false,
        value: "title",
        values: "actions",
      },

      {
        text: "Url",
        value: "url",
        sortable: false,
      },
      { text: "Subtitle", value: "subtitle", sortable: false },
      { text: "isbn13", value: "isbn13", sortable: false },
      { text: "Price", value: "price", sortable: false },
      { text: "Url", value: "url", sortable: false },
    ],
    desserts: [],
  }),
  created() {
    this.fetch();
  },
  methods: {
  
    //conexion con la api
    fetch() {
      var search = this.search;
  
      axios
        .get("https://api.itbook.store/1.0/search/" + search)
        .then((result) => {
          console.log(result);
          var res = result.data.books;
          res.forEach((element) => {
            this.url = element.image;
            this.desserts.push(element);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      this.page = 1;
      this.fetch();
    },
  },
};
</script>
