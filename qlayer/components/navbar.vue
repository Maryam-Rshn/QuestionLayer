<template>
  <div class="container">
    <div class="logo">
        QLayer
    </div>
    <div class="maps">
        <h3>About</h3>
        <h3>FAQ</h3>
        <div class="account">
            Login
        </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.container {
    padding: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.logo {
    font-size: 22px;
    font-weight: 700;
    color: #2C3333;
}
.maps {
    display: flex;
    justify-content: space-between;
    gap: 30px;
    align-items: center;
}
.account {
    background-color: #395B64;
    color: white;
    border-radius: 20px;
    padding: 0 18px;
    display: flex;
    align-items: center;
    height: 40px;
    cursor: pointer;
}
h3 {
    font-size: 17px;
    font-weight: 600;
    cursor: pointer;
    color: #2C3333;
}
h3:hover {
    color: #395B64;
}
.account:hover {
    color: #395B64;
    background-color: #E7F6F2;
}
</style>