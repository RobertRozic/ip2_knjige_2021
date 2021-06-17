<template>
  <v-form @submit.prevent="submit">
    <v-container>
      <v-row>
        <v-col
            cols="12"
        >
          <v-text-field
              v-model="title"
              :counter="10"
              label="Title"
              required
          ></v-text-field>
        </v-col>

        <v-col
            cols="12"
        >
          <v-text-field
              v-model="author"
              :counter="10"
              label="Author"
              required
          ></v-text-field>
        </v-col>

        <v-col
            cols="12"
        >
          <v-text-field
              v-model="price"
              label="Price"
              type="number"
              required
          ></v-text-field>
        </v-col>

        <v-col cols="12">
          <v-btn type="submit" variant="primary">
            Submit
          </v-btn>
        </v-col>

      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  data () {
    return {
      title: '',
      author: '',
      price: ''
    }
  },
  methods: {
    submit: function () {
      this.$emit('success')
    }
  }
}
</script>
