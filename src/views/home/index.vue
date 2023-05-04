<template>
  

  <div class='building'>
    <div>
      <div class="home">
        欢迎使用，校园二手交易小程序后台管理系统
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "index"
};
</script>

<style lang="scss" scoped>
.building{
  background:url("../../assets/login_bkgd.jpg");
  width:100%;
  height:100%;
  position:fixed;
  background-size:100% 100%;
  .home{
    padding: 40px;
    font-size: 30px;
    font-weight: bold;
  }

}

</style>