<template>
  <div class="page-search">
    <div class="columns is-multiline">
      <div class="is-12 column">
        <h1 class="title">Search</h1>
        <h2 class="is-size-5 has-text-centered">search term was '{{ query }}'</h2>
      </div>

      <Productbox
        v-for="product in products"
        v-bind:product="product"
        v-bind:key="product.id"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Productbox from "@/components/Productbox";
export default {
  name: "Search",

  data() {
    return {
      query: "",
      products: [],
    };
  },

  components: {
    Productbox,
  },
  mounted() {
    this.performSeach();
  },

  methods: {
    performSeach() {
      this.query = this.$route.query.query;
      this.$store.commit("SetIsLoading", true);
      axios
        .post("http://127.0.0.1:8000/api/v1/products/search", {
          query: this.query,
        })
        .then((res) => {
          this.products = res.data;
        })
        .catch((err) => {
          console.error(err);
        });
      this.$store.commit("SetIsLoading", false);
    },
  },
};
</script>