<template>
  <div class="food-detail">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb bg-white">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li
                class="breadcrumb-item text-dark font-weight-bold active"
                aria-current="page"
              >
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col-md-6">
          <img
            class="img-fluid shadow rounded"
            :src="'../assets/image/' + product.gambar"
          />
        </div>
        <div class="col-md-6">
          <h2 class="text-dark font-weight-bold">{{ product.nama }}</h2>
          <hr />
          <h4 class="text-dark">
            Harga : <strong>Rp {{ product.harga }}</strong>
          </h4>
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan" class="text-dark font-weight-bold"
                >Jumlah Pesana</label
              >
              <input
                type="number"
                class="form-control"
                min="1"
                placeholder="jumlah"
                v-model="pesan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan" class="text-dark font-weight-bold"
                >Keterangan</label
              >
              <textarea
                class="form-control"
                placeholder="ketarangan"
                v-model="pesan.keterangan"
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cash></b-icon-cash>
              Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      pesan: {},
    };
  },
  methods: {  
    setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlah_pemesanan) {
        this.pesan.products = this.product;
        axios
          .post("http://localhost:3000/keranjang", this.pesan)
          .then(() => {
            this.$router.push({ path : "/keranjang" })
            this.$toast.success("Success", {
              type: "success",
              position: "top-right",
              duration: "3000",
              dismissible: true,
            });
          })
          .catch((error) => console.log(error));
      } else{
         this.$toast.error("Harus di Isi Semua", {
              type: "error",
              position: "top-right",
              duration: "3000",
              dismissible: true,
      });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then(
        (response) => this.setProduct(response.data)
        // console.log("berhasil:" , response);
      )
      .catch((error) => console.log("Gagal:", error));
  },
};
</script>

<style>
</style>