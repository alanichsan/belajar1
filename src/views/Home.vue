<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />

      <div class="row mt-4">
        <div class="col">
          <h2>Best <strong>Foods</strong></h2>
        </div>
      </div>

      <div class="col">
        <router-link to="/foods" class="btn btn-success">
          <b-icon-eye></b-icon-eye> Lihat Semua</router-link
        >
      </div>

      <div class="row mt-5 mb-5">
        <div
          class="col-md-4 mt-4"
            v-for="product in products"
            :key="product.id" 
        >
          <!-- <div class="col-md-4 mt-4"> -->
          <!-- <CardProduct /> -->
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Navbar,
    Hero,
    CardProduct,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/best-products")
      .then(
        (response) => this.setProducts(response.data)
        // console.log("berhasil:" , response);
      )
      .catch((error) => console.log("Gagal:", error));
  },
};
</script>
