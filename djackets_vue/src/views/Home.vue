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

      <!-- Product -->
      <ProductBox 
        v-for="(product, index) in latestProducts"
        :key="index"
        :product="product"
      />
      
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import ProductBox from '../components/ProductBox.vue'
export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Home | Djackets'
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