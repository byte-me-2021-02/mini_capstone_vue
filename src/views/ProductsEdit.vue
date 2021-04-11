<template>
  <div class="products-edit">
    <h1>{{ message }}</h1>
    <form v-on:submit.prevent="submit">
      
      <p>name: <input type="text" v-model="product.name"></p>
      <p>price: <input type="text" v-model="product.price"></p>
      <p>description: <input type="text" v-model="product.description"></p>      
      <p>quantity: <input type="text" v-model="product.quantity"></p>
      <p></p>
       supplier:
      <select v-model="product.supplier_id">
        <option v-bind:value="supplier.id" v-for="supplier in suppliers">{{supplier.name}}</option>        
      </select>
      <input type="submit" value="Edit the product">
    </form>
  </div>
</template>

<style>
</style>

<script>

// make a form
// prepopulate the form
  // getting data from the api based on the id in th url (show)
  // add that data to the form
// when the user submits the form, send form data to rails
// redirect to the show page

import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js! edit",
      product: {},
      suppliers: []
    };
  },
  created: function() {
    axios.get(`/api/products/${this.$route.params.id}`).then(response => {
      console.log(response.data)
      this.product = response.data;
    })

    // get the daata on the suppliers
    axios.get('/api/suppliers').then(response => {
      console.log('suppliers')
      console.log(response.data)
      this.suppliers = response.data;
    })
  },
  methods: {
    submit: function() {
      console.log('form was submitted')
      // collect the data from the form
      var params = {
        name: this.product.name,
        price: this.product.price,        
        description: this.product.description,
        supplier_id: this.product.supplier_id,
        quantity: this.product.quantity,
      }

      // send that data to rails to the update action
      axios.patch(`/api/products/${this.$route.params.id}`, params).then(response => {
        console.log(response.data);
        this.$router.push(`/products/${this.$route.params.id}`)
      })
    }
  }
};
</script>

