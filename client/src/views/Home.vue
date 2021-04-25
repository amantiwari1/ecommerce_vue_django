<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <p class="title mb-6">Welcome to Djackaget</p>
        <p class="subtitle">the best jacket store online</p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Latest Product</h2>
      </div>
      <Productbox
        v-bind:product="product"
        v-for="product in latestProducts"
        v-bind:key="product.id"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
axios.defaults.baseURL = 'http://127.0.0.1:8000'

import Productbox from "@/components/Productbox";
export default {
  name: "Home",

  data() {
    return {
      latestProducts: [],
    };
  },
  components: {
    Productbox, 
  },
  mounted() {
    this.getLatestProduct();
    document.title = "Home | Djackter";
  },
  methods: {
    async getLatestProduct() {
      this.$store.commit("SetIsLoading", true);

      await axios
        .get("http://127.0.0.1:8000/api/v1/latest-products/")
        .then((res) => {
          this.latestProducts = res.data;
          console.log(res.data);
        })
        .catch((err) => {
          console.error(err);
        });

      this.$store.commit("SetIsLoading", false);
    },
  },
};
</script>
