<template>
  <div>
    <div class="container">
      <div class="main-panel">
        <img
          class="product-image"
          :src="product.imageUrl"
          :alt="product.name"
        />
      </div>
      <div class="side-panel">
        <p class="name">{{ product.name }}</p>
        <p class="price">{{ product.price }}</p>
        <div class="cartWrap">
          <button type="button" @click="addToCart">카트에 담기</button>
          <!-- 
          <NuxtLink to="/cart">
            <button type="button">addToCart</button>
          </NuxtLink>
           -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { createCartItem, fetchProductById } from '@/api/index'

export default {
  async asyncData({ params }) {
    const response = await fetchProductById(params.id)
    const product = response.data
    return { product }
  },
  head() {
    return {
      title: `Shopping Item Detail - ${this.product.name}`,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: `이 상품의 가격은 ${this.product.name} 입니다.`,
        },
        {
          hid: 'price',
          name: 'price',
          content: `이 상품의 가격은 ${this.product.price} 입니다.`,
        },
      ],
    }
  },
  methods: {
    async addToCart() {
      await createCartItem(this.product)

      this.$store.commit('addCartItem', this.product)
      this.$router.push('/cart')
    },
  },
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  margin: 2rem 0;
}
.product-image {
  width: 500px;
  height: 375px;
}
.side-panel {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 220px;
  text-align: center;
  padding: 0 1rem;
}
</style>
