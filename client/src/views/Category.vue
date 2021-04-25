<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
      </div>
      <Productbox
        v-for="product in category.products"
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
  nme: "Category",
  data() {
    return {
      category: {
        products: [],
      },
    };
  },

  components: {
    Productbox,
  },

  mounted() {
    this.getCategory();
  },

  watch: {
    $route(to, from) {
      if (to.name === 'Category') {
        this.getCategory();
      }
    },
  },

  methods: {
    async getCategory() {
      this.$store.commit("SetIsLoading", true);

      const category_slug = this.$route.params.category_slug;

      await axios
        .get(`http://localhost:8000/api/v1/products/${category_slug}`)
        .then((res) => {
          this.category = res.data;
          document.title = res.data.name + " | Djackter";
        })
        .catch((err) => {
          console.error(err);
        });
      this.$store.commit("SetIsLoading", false);
    },
  },
};
</script>