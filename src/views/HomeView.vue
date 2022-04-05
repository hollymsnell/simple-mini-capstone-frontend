<template>
  <div class="home">
    <h1 v-bind:class="className1">{{ message }} count: {{ products.length }}</h1>
    <h2 v-bind:class="className2">New Product</h2>
    Name
    <input type="text" v-model="productName" />
    Price
    <input type="number" v-model="productPrice" />
    Description
    <input type="text" v-model="productDescription" />
    Image_url
    <input type="text" v-model="productImage_url" />
    <button v-on:click="createProduct()">Create</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h3>{{ product.name }}</h3>
      <p>${{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" style="max-width: 200px" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "The Re-tail Store",
      products: [],
      className1: "green",
      className2: "blue",
      errors: [],
      productName: "",
      productPrice: 0,
      productDescription: "",
      productImage_url: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts() {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
      });
    },
    createProduct() {
      var params = {
        name: this.productName,
        price: this.productPrice,
        description: this.productDescription,
        image_url: this.productImage_url,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
  },
};
</script>

<style>
.green {
  color: green;
}
.blue {
  color: blue;
}
</style>
