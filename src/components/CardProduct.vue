<template>
  <div class="card shadow card-product">
    <img
      class="card-img-top"
      :src="'assets/image/' + product.gambar"
      alt="Card image cap"
    />
    <div class="card-body">
      <h5 class="card-title font-weight-bold">{{ product.nama }}</h5>
      <h5 class="card-text"><strong>Harga</strong> : Rp {{ product.harga }}</h5>
      <router-link class="btn btn-success float-right"  :to=" '/foods/' +product.id  "
        ><b-icon-cart class="mr-2"></b-icon-cart>Buy</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
};
</script>

<style>
</style>