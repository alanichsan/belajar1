<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col">
          <h2 class="text-center">Daftar <strong>Makanan</strong></h2>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control"
              placeholder="Cari Makanan"
              aria-label="Search"
              aria-describedby="basic-addon1"
              v-model="search"
              @keyup="cariMakanan"
            />
            <div class="input-group-prepend">
              <span class="input-group-text" id="basic-addon1"><b-icon-search class="mr-2"></b-icon-search></span>
            </div>    
          </div>
        </div>
      </div>

      <div class="row mb-5">
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
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search:'',
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
      cariMakanan() {
    axios
      .get("http://localhost:3000/products?q="+this.search)
      .then(
        (response) => this.setProducts(response.data)
        // console.log("berhasil:" , response);
      )
      .catch((error) => console.log("Gagal:", error));
  },
  },

  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then(
        (response) => this.setProducts(response.data)
        // console.log("berhasil:" , response);
      )
      .catch((error) => console.log("Gagal:", error));
  },
};
</script>

<style>
</style>