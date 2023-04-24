<template>
  <div class="container">
    <Navbar/>
    <Hero/>
    <QuestionLayer/>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  components: {
    Navbar:() => import('@/components/navbar.vue'),
    Hero:() => import('@/components/hero.vue'),
    QuestionLayer:() => import('@/components/QuestionLayer.vue'),
  }
}
</script>
<style>
.container {
  padding: 20px 110px 0;
}
</style>
