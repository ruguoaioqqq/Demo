<template>
  <div>
    <div class="img">
      <img src="../assets/logo2.jpg">
    </div>
    <div class="user">{{userName}}</div>
    <div class="welcome">欢迎来到个人中心</div>
    <div class="add">
      <router-link :to="{name:'add'}">添加便笺</router-link>
    </div>
  </div>
</template>
<script>
export default {
  name: "User",
  data: function() {
    return {
      userName: localStorage.getItem("name")
    };
  }
};
</script>
<style scoped lang="scss">
.img {
  margin: 30px;
  text-align: center;
  img{
    width: 220px;
    height: 150px;
  }
}
.user {
  font-weight: bold;
}
.welcome {
  text-align: center;
  margin: 5px;
}
.add {
  text-align: center;
  margin: 20px;
  margin-top: 40px;
  height: 35px;
  line-height: 35px;
  background-color: #42b983;
  border: 1px solid #42b983;
  border-radius: 8px;
  a {
    text-decoration: none;
    color: #fff;
    display: block;
  }
}
</style>
