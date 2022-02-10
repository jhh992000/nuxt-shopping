<template>
  <div class="app">
    <main>
      <div>
        <SearchInput
          v-model="searchKeyword"
          @search="searchProducts"
        ></SearchInput>

        <!-- 위와 같은 기능을 함
        <SearchInput
          :search-keyword="searchKeyword"
          @input="updateSearchKeyword"
        ></SearchInput>
         -->
      </div>
      <ul>
        <li
          v-for="product in products"
          :key="product.id"
          class="item flex"
          @click="moveToDetailPage(product.id)"
        >
          <img
            class="product-image"
            :src="product.imageUrl"
            :alt="product.name"
            width="100"
          />
          <p>{{ product.name }}</p>
          <span>{{ product.price }}</span>
        </li>
      </ul>
    </main>
    <div class="cart-wrapper">
      <button type="button" class="btn" @click="routeToCartPage">
        장바구니로 이동
      </button>
      <!-- 
      <NuxtLink to="/cart">
        <button type="button" class="btn">장바구니로 이동</button>
      </NuxtLink>
       -->
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import SearchInput from '@/components/SearchInput.vue'
import { fetchProductByKeyword } from '~/api'

export default {
  name: 'IndexPage',
  components: { SearchInput },

  async asyncData() {
    const response = await axios.get('http://localhost:3000/products')
    const products = response.data.map((item) => {
      return {
        ...item,
        imageUrl: `${item.imageUrl}?random=${Math.random()}`,
      }
    })
    return { products }
  },

  data() {
    return {
      searchKeyword: '',
    }
  },

  methods: {
    moveToDetailPage(id) {
      this.$router.push(`detail/${id}`)
    },
    updateSearchKeyword(keyword) {
      this.searchKeyword = keyword
    },
    async searchProducts() {
      const response = await fetchProductByKeyword(this.searchKeyword)
      this.products = response.data.map((item) => {
        return {
          ...item,
          imageUrl: `${item.imageUrl}?random=${Math.random()}`,
        }
      })
    },
    routeToCartPage() {
      this.$router.push('/cart')
    },
  },
}
</script>

<style scoped>
.flex {
  display: flex;
  justify-content: center;
}
.item {
  display: inline-block;
  width: 400px;
  height: 300px;
  text-align: center;
  margin: 0 0.5rem;
  cursor: pointer;
}
.product-image {
  width: 400px;
  height: 250px;
}
.app {
  position: relative;
}
.cart-wrapper {
  position: sticky;
  float: right;
  bottom: 50px;
  right: 50px;
}
.cart-wrapper .btn {
  display: inline-block;
  height: 40px;
  font-size: 1rem;
  font-weight: 500;
}
</style>
