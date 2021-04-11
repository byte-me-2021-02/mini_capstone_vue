<template>
  <div class="products-show">
    <!-- <h1>{{ product }}</h1> -->
    <h1>name: {{ product.name }}</h1>
    <p>description: {{ product.description }}</p>
    <!-- <p>images: {{ product.images[0].url }}</p>     -->
    <img v-for="image in product.images" v-bind:src="image.url">
    <p>supplier: {{ product.supplier_id }}</p>
  </div>
</template>

<style>
</style>

<script>
// what needs to happen for a show page?
// get data from the api
// based on the id in the url
// show that data to the user
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to to the show!",
      product: {}
    };
  },
  created: function() {
    console.log('in created')
    axios.get(`/api/products/${this.$route.params.id}`).then(response => {
      console.log(response.data)
      this.product = response.data;
    })
  },
  methods: {}
};
</script>

