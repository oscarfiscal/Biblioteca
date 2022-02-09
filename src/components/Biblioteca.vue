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
          <v-chip
            class="open-modal"
            data-open="modal2"
            @click="modal(item)"
            white
          >
            {{ item.title }}
          </v-chip>
        </template>
      </v-data-table>

      <div class="modal" id="modal2" data-animation="mixInAnimations">
        <div class="modal-dialog">
          <header class="modal-header">
           {{title}}
            <button class="close-modal" aria-label="close modal" data-close>
              ✕
            </button>
          </header>
          <section class="modal-content">
          
            <v-img
              :src="image"
              height="125"
              class="grey darken-4"
            ></v-img>
          </section>
        </div>
      </div>
    </v-card>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Biblioteca",
  data: () => ({
    id: "",
    page: 1,
    pages: 1,
    search: "",
    image:"",
    title:"",
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
    modal(item) {
      console.log(item.isbn13);
       axios
        .get("https://api.itbook.store/1.0/books/" + item.isbn13)
        .then((response) => {
          this.image = response.data.image;
          this.title= response.data.title;
          console.log(response.data);

        })
        .catch((error) => {
          console.log(error);
        });
    

      const openEls = document.querySelectorAll("[data-open]");
      const closeEls = document.querySelectorAll("[data-close]");
      const isVisible = "is-visible";

      for (const el of openEls) {
        el.addEventListener("click", function () {
          const modalId = this.dataset.open;
          document.getElementById(modalId).classList.add(isVisible);
        });
      }

      for (const el of closeEls) {
        el.addEventListener("click", function () {
          this.parentElement.parentElement.parentElement.classList.remove(
            isVisible
          );
        });
      }

      document.addEventListener("click", (e) => {
        if (e.target == document.querySelector(".modal.is-visible")) {
          document
            .querySelector(".modal.is-visible")
            .classList.remove(isVisible);
        }
      });

      document.addEventListener("keyup", (e) => {
        // if we press the ESC
        if (e.key == "Escape" && document.querySelector(".modal.is-visible")) {
          document
            .querySelector(".modal.is-visible")
            .classList.remove(isVisible);
        }
      });
    },

    //conexion con la api
    fetch() {
      var search = this.search;

      axios
        .get("https://api.itbook.store/1.0/search/" + search)
        .then((result) => {
          console.log(result);
          var res = result.data.books;
          res.forEach((element) => {
            this.id = element.isbn13;

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
<style>
/* RESET RULES 
–––––––––––––––––––––––––––––––––––––––––––––––––– */
:root {
  --lightgray: #5a5555;
  --blue: steelblue;
  --white: #fff;
  --black: rgba(0, 0, 0, 0.8);
  --bounceEasing: cubic-bezier(0.51, 0.92, 0.24, 1.15);
}

.open-modal {
  font-weight: bold;
  background: var(--blue);
  color: var(--white);
  padding: 0.75rem 1.75rem;
  margin-bottom: 1rem;
  border-radius: 5px;
}

/* MODAL
–––––––––––––––––––––––––––––––––––––––––––––––––– */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: var(--black);
  cursor: pointer;
  visibility: hidden;
  opacity: 0;
  transition: all 0.35s ease-in;
}

.modal.is-visible {
  visibility: visible;
  opacity: 1;
}

.modal-dialog {
  position: relative;
  max-width: 800px;
  max-height: 80vh;
  border-radius: 5px;
  background: var(--white);
  overflow: auto;
  cursor: default;
}

.modal-dialog > * {
  padding: 1rem;
}

.modal-header,
.modal-footer {
  background: var(--lightgray);
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.modal-header .close-modal {
  font-size: 1.5rem;
}

.modal p + p {
  margin-top: 1rem;
}

/* ANIMATIONS
–––––––––––––––––––––––––––––––––––––––––––––––––– */
[data-animation] .modal-dialog {
  opacity: 0;
  transition: all 0.5s var(--bounceEasing);
}

[data-animation].is-visible .modal-dialog {
  opacity: 1;
  transition-delay: 0.2s;
}

[data-animation="slideInOutDown"] .modal-dialog {
  transform: translateY(100%);
}

[data-animation="slideInOutTop"] .modal-dialog {
  transform: translateY(-100%);
}

[data-animation="slideInOutLeft"] .modal-dialog {
  transform: translateX(-100%);
}

[data-animation="slideInOutRight"] .modal-dialog {
  transform: translateX(100%);
}

[data-animation="zoomInOut"] .modal-dialog {
  transform: scale(0.2);
}

[data-animation="rotateInOutDown"] .modal-dialog {
  transform-origin: top left;
  transform: rotate(-1turn);
}

[data-animation="mixInAnimations"].is-visible .modal-dialog {
  animation: mixInAnimations 2s 0.2s linear forwards;
}

[data-animation="slideInOutDown"].is-visible .modal-dialog,
[data-animation="slideInOutTop"].is-visible .modal-dialog,
[data-animation="slideInOutLeft"].is-visible .modal-dialog,
[data-animation="slideInOutRight"].is-visible .modal-dialog,
[data-animation="zoomInOut"].is-visible .modal-dialog,
[data-animation="rotateInOutDown"].is-visible .modal-dialog {
  transform: none;
}

@keyframes mixInAnimations {
  0% {
    transform: translateX(-100%);
  }

  10% {
    transform: translateX(0);
  }

  20% {
    transform: rotate(20deg);
  }

  30% {
    transform: rotate(-20deg);
  }

  40% {
    transform: rotate(15deg);
  }

  50% {
    transform: rotate(-15deg);
  }

  60% {
    transform: rotate(10deg);
  }

  70% {
    transform: rotate(-10deg);
  }

  80% {
    transform: rotate(5deg);
  }

  90% {
    transform: rotate(-5deg);
  }

  100% {
    transform: rotate(0deg);
  }
}
</style>
