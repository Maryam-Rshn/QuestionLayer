<template>
  <div class="hero_section">
    <div class="hero_text">
      <h1>make your own <span>Question Layer</span></h1>
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor</p>
    </div>
    <img src="@/assets/undraw_questions_re_1fy7 1.svg" alt="">
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss">
.hero_section {
    padding: 50px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    img {
      width: 650px;
    }
}
.hero_text {
  h1 {
    font-size: 50px;
    width: 400px;
    line-height: 60px;
    // text-align: center;
    color: #2C3333;
    
    span {
      color: #395B64;
    }
  }
  p {
    // text-align: center;
    width: 400px;
    font-size: 17px;
    font-weight: 700;
    line-height: 25px;
  }
}
</style>