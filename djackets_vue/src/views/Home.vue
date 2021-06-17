<template>
  <div class="home">
    <section class="mb-6 hero is-medium is-dark">
      <div class="hero-body has-text-centered">
        <p class="mb-6 title">Welcome to Djacket</p>
        <p class="subtitle">The best djacket store online</p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size2 has-text-centered">Latest Products</h2>
      </div>

      <div 
        class="column is-3" 
        v-for="(product, index) in latestProducts"
        :key="index"
      >
        <div class="box">
          <figure class="mb-4 image">
            <img :src="product.get_thumbnail">
          </figure>

          <h3 class="is-size-4">{{product.name}}</h3>
          <p class="is-size-6 has-text-grey">${{product.price}}</p>

          <router-link :to="product.get_absolute_url" class="mt-4 button is-dark">View Details</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
  },
  mounted() {
    this.getLatestProducts()
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)
      await axios
        .get('api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })
      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

<style scoped> 
.image {
  margin-top: -1.25rem;
  margin-left: -1.25rem;
  margin-right: -1.25rem;
}
</style>