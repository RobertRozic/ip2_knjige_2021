<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-dialog
            v-model="addDialog"
            width="500"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
                color="blue lighten-2"
                dark
                v-bind="attrs"
                v-on="on"
            >
              Add new book
            </v-btn>
          </template>

          <v-card>
            <v-card-title class="text-h5 grey lighten-2">
              Add new book
            </v-card-title>

            <v-card-text>
              <new-form @success="addDialog = false; successAlert=true;"></new-form>
            </v-card-text>

            <v-divider></v-divider>
          </v-card>
        </v-dialog>
      </v-col>
      <v-col cols="12">
        <v-alert type="success"
                 v-model="successAlert"
                 dismissible>
          Successfuly saved.
        </v-alert>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="6">
        <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search by title"
            single-line
            hide-details
            :loading="isLoading"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="6">
        <v-text-field
            v-model="author"
            append-icon="mdi-magnify"
            label="Search by author"
            single-line
            hide-details
            :loading="isLoadingAuthor"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col
          v-for="book in books"
          :key="book.title"
          cols="12"
          md="3"
      >
        <book :book="book"></book>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-pagination
            v-model="page"
            total-visible="10"
            :length="Math.ceil(totalBooks/perPage)"
        ></v-pagination>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
//import HelloWorld from '../components/HelloWorld'
import Book from "../components/Book";
import NewForm from "../components/NewForm";
export default {
  name: 'Home',

  components: {
    NewForm,
    Book
  },

  data() {
    return {
      test: 'Testna varijabla',
      books: [],
      page: 1,
      totalBooks: 0,
      perPage: 20,
      search: '',
      author: '',
      isLoading: false,
      isLoadingAuthor: false,
      addDialog: false,
      successAlert: false
    }
  },

  created() {
    console.log('created')
    this.getData();
  },

  methods: {
    getData() {
      let api = "https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json"
      this.axios.get(api, {
        params: {
          'api-key': 'jGZL1u0s3GnGPUHbWdzefYCYQGDX0zus',
          'offset': this.perPage * (this.page - 1),
          'title': this.search,
          'author': this.author
        }
      }).then((response) => {
        console.log(response.data)
        this.books = response.data.results
        this.totalBooks = response.data.num_results
        this.isLoading = false
        this.isLoadingAuthor = false
      })
    },
    fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId)
      this._searchTimerId = setTimeout(() => {
        this.getData()
      }, 500) /* 500ms throttle */
    },
    searchEntries() {
      this.books = []
      this.page = 1
      this.fetchEntriesDebounced()
    }
  },

  watch: {
    page: function() {
      this.getData();
    },
    search (val) {
      if (!val) {
        return
      }
      this.isLoading = true
      this.searchEntries()
    },
    author (val) {
      if (!val) {
        return
      }
      this.isLoadingAuthor = true
      this.searchEntries();
    }
  }
}
</script>
