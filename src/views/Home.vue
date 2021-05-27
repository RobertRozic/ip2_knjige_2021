<template>
  <v-container>
    <v-row>
      <v-col
          v-for="book in books"
          :key="book.title"
          cols="4"
      >
        <v-card height="200">
          <h3>{{ book.title }}</h3>
          <h5>{{ book.author }}</h5>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  //import HelloWorld from '../components/HelloWorld'

  export default {
    name: 'Home',

    data() {
      return {
        test: 'Testna varijabla',
        books: []
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
            'api-key': 'jGZL1u0s3GnGPUHbWdzefYCYQGDX0zus'
          }
        }).then((response) => {
          console.log(response.data)
          this.books = response.data.results
        })
      }
    }
    //components: {
    //  HelloWorld,
    //},


  }
</script>
