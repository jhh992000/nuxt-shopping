<template>
  <div>
    <header class="title">
      <NuxtLink to="/" class="logo">Nuxt Shopping</NuxtLink>
    </header>
    <Nuxt />
    <!-- 
      <hr />
      <Nuxt />
      <hr />
      <router-view></router-view>
       -->
  </div>
</template>

<script>
export default {
  name: 'DefaultPage',
}
</script>

<style scoped>
/* 헤더 관련 스타일 */
header {
  height: 60px;
  display: flex;
  align-items: center;
  padding: 0 0.5rem;
}
.logo {
  font-weight: 900;
}
.logo:visited {
  color: inherit;
}
</style>
