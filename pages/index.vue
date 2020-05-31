<template>
  <section class="main-content">
    <div class="container ">
      <div class="admin-header mb-2">
        <h1>All Product</h1>
        <nuxt-link to="/new-product" class="btn btn-primary">Add new product</nuxt-link>
        <nuxt-link to="/new-category" class="btn btn-primary">Add new Category</nuxt-link>
        <nuxt-link to="/new-owner" class="btn btn-primary">Add new Oner</nuxt-link>
      </div>
      <b-row class="mt-3">
        <b-col v-for="product in products" :key="product._id" sm="4">
          <b-card
            :title="product.title"
            :img-src="product.productImage"
            img-alt="Image"
            img-top
            tag="article"
            class="mb-2"
          >
            <b-form-rating
              v-model="value"
              variant="warning"
              no-border
            ></b-form-rating>
            <b-card-text>
              {{ product.description }}
            </b-card-text>
            <h4 class="price">${{ product.price }}</h4>
            <b-button href="#" variant="primary">update</b-button>
            <b-button href="#" variant="primary">Delete</b-button>
          </b-card>
        </b-col>
      </b-row>
    </div>
  </section>
</template>

<script>
import Logo from "~/components/Logo.vue";

export default {
  async asyncData ({ $axios }) {
    try {
      const response = await $axios.$get('http://localhost:8000/api/products');
      return {
        products: response.products    
      }
    } catch (error) {
      console.error(error);
    }
  },
  data() {
      return {
        value: null,
      }
  },
  components: {
    Logo
  }
};
</script>

<style></style>
