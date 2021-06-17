<template>
  <v-card height="200"
          class="book text--white pa-2 d-flex flex-column justify-space-between">
    <h3 class="text-center book-title">{{ book.title }}</h3>
    <h6>{{ book.author }}</h6>
    <h6>Price: {{ book.price }}</h6>
    <v-dialog
        v-model="dialog"
        width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
            color="primary lighten-2"
            dark
            v-bind="attrs"
            v-on="on"
        >
          Details
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          {{ book.title }}
        </v-card-title>

        <v-card-text>
          {{ book.description }}
        </v-card-text>

        <v-divider></v-divider>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
export default {
  props: ['book'],
  data () {
    return {
      dialog: false
    }
  }
}
</script>

<style>
.book {
  background: #2980b9 !important;
  color: white !important;
}

.book-title {
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
</style>
