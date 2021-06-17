<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">{{category.name}}</h2>
      </div>

      <ProductBox 
        v-for="(product, index) in category.products"
        :key="index"
        :product="product"
      />


    </div>
  </div>
</template>

<script>
import axios from 'axios'
import {toast} from 'bulma-toast'
import ProductBox from '../components/ProductBox.vue'

export default {
  components: { ProductBox },
  name: 'Category',
  data() {
    return {
      category: {
        products: []
      }
    }
  },
  mounted() {
    this.getCategory()
  },
  watch: {
    $route(to, from) {
      if (to.name === 'Category') {
        this.getCategory()
      }
    }
  },
  methods: {
    async getCategory() {
      const categorySlug = this.$route.params.category_slug

      this.$store.commit('setIsLoading', true)

      axios
        .get(`api/v1/products/${categorySlug}`)
        .then(response => {
          this.category = response.data
          document.title = this.category.name + ' | Djackets'
        })
        .catch(error => {
          console.log(error)
          
          toast({
            message: 'Something went wrong. Please try again.',
            type: 'is-danger',
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
            position: 'bottom-right'
          })
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>