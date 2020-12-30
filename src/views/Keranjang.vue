<template>
  <div class="keranjang">
    <Navbar  :updateKeranjang="keranjangs"/>
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <!-- Card -->
      <div class="card wish-list mb-3">
        <div
          v-for="(keranjang, index) in keranjangs"
          :key="keranjang.id"
          class="card-body"
        >
          <p>
            # <strong>{{ index + 1 }}</strong>
          </p>
          <div class="row mb-4">
            <div class="col">
              <img
                class="img-fluid shadow rounded"
                :src="'../assets/image/' + keranjang.products.gambar"
              />
            </div>
            <div class="col-md-7 col-lg-9 col-xl-9">
              <div>
                <div class="d-flex justify-content-between mt-3">
                  <div>
                    <h5>
                      <strong> {{ keranjang.products.nama }}</strong>
                    </h5>
                  </div>
                  <div>
                    <p class="mb-0">
                      <span
                        ><strong>{{ keranjang.jumlah_pemesanan }}</strong></span
                      >
                    </p>
                  </div>
                </div>
                <div
                  class="d-flex justify-content-between align-items-center mt-3"
                >
                  <div>
                    <p class="mb-0">
                      <span
                        ><strong
                          >{{
                            keranjang.keterangan ? keranjang.keterangan : " - "
                          }}
                        </strong></span
                      >
                    </p>
                  </div>
                  <p class="mb-0">
                    <span
                      ><strong>Rp. {{ keranjang.products.harga }}</strong></span
                    >
                  </p>
                </div>
                <hr class="mb-4" />
                <div
                  class="d-flex justify-content-between align-items-center mt-3"
                >
                  <div>
                    <a
                      @click="hapusdata(keranjang.id)"
                      type="button"
                      class="nav-link small mr-3 text-danger"
                    >
                      <b-icon-trash-fill></b-icon-trash-fill>
                      Remove item
                    </a>
                  </div>
                  <p class="mb-0">
                    <span
                      ><strong
                        >Rp.
                        {{
                          keranjang.products.harga * keranjang.jumlah_pemesanan
                        }}</strong
                      ></span
                    >
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="card mb-5 mt-2">
        <div class="card-body">
          <div class="d-flex justify-content-between align-items-center">
            <div>
              <h5>Total Harga :</h5>
            </div>
            <div>
              <h5>
                <strong>Rp. {{ totalHarga }}</strong>
              </h5>
            </div>
          </div>
        </div>
      </div>
      <!-- Card -->
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusdata(id) {
      axios
        .delete("http://localhost:3000/keranjang/" + id)
        .then(() => {
          this.$toast.error("Success Deleted", {
            type: "error",
            position: "top-right",
            duration: "3000",
            dismissible: true,
          });

          // Update data
          axios
            .get("http://localhost:3000/keranjang")
            .then(
              (response) => this.setKeranjangs(response.data)
              // console.log("berhasil:" , response);
            )
            .catch((error) => console.log("Gagal:", error));
        })
        .catch((error) => console.log("Gagal:", error));
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjang")
      .then(
        (response) => this.setKeranjangs(response.data)
        // console.log("berhasil:" , response);
      )
      .catch((error) => console.log("Gagal:", error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>