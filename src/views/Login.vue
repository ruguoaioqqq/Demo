<template>
  <div>
    <!-- <button @click="logins()">登录</button>
    <button>注册</button>-->
    <form v-if="!isReg">
      <div class="header">欢迎登录</div>
      <div class="form">
        <label>用户名:</label>
        <input type="text" v-model="name" />
      </div>
      <div class="form">
        <label>密码:</label>
        <input type="password" v-model="password" />
      </div>
      <div class="buttons">
        <button type="button" class="first" @click="login()">登录</button>
        <button type="button" class="second" @click="reg()">注册</button>
      </div>
    </form>

    <form v-else>
      <div class="header">请输入以下信息</div>
      <div class="form">
        <label>用户名:</label>
        <input type="text" v-model="name" />
      </div>
      <div class="form">
        <label>密码:</label>
        <input type="password" v-model="password" />
      </div>
      <div class="form">
        <label>确认密码:</label>
        <input type="password" v-model="repeat" />
      </div>
      <div class="buttons">
        <button type="button" class="first" @click="addUser()">确定</button>
        <button type="button" class="second" @click="cancel()">取消</button>
      </div>
    </form>
    <Button type="success">Success</Button>
    <Button type="warning">Warning</Button>
    <!-- <Switch size="large" />
    <Switch />
    <Switch size="small" /> -->
     <Input v-model="value" placeholder="Enter something..." style="width: 300px" />
  </div>
</template>

<script>
import store from "@/store";
import { createHash } from "crypto";

export default {
  name: "Login",
  data: function() {
    return {
      isReg: false,
      value:'',
      name: "",
      password: "",
      repeat: ""
    };
  },
  // 监听
  watch: {
    name: function(newV, oldV) {
      if (newV == "user") {
        console.log("进去了");
      }
    }
  },
  mounted() {
    store.commit("getLocalUsers");
    console.log("11111111111");
    // axios接口get请求
    axios.get("http://jsonplaceholder.typicode.com/todos/10").then(res => {
      console.log("get", res.data);
    });
    // axios接口post请求
    axios({
      method: "post",
      url: "https://miniappshome.cn/civilAviation//data/openId",
      data: {
        code: "001BL5JV1AymaY0pfZJV1OslJV1BL5JV"
      },
      header: {
        "content-type": "application/x-www-form-urlencoded"
      }
    }).then(res => {
      console.log("post", res);
    });
  },

  // 计算属性
  computed: {
    users() {
      return store.state.users;
    },
    checkUser() {
      return this.users.find(
        user => user.name === this.name && user.password === this.password
      );
    }
  },
  // 方法
  methods: {
    //登录
    login: function() {
      if (this.name === "" || this.password === "") {
        alert("请输入用户名和密码");
      } else if (this.checkUser) {
        sessionStorage.setItem("currentUser", this.checkUser.name);
        this.name = "";
        this.password = "";
        this.$router.push("/home/user");
      } else {
        alert("输入的账号密码错误！");
      }
    },
    //注册
    reg: function() {
      this.name = "";
      this.password = "";
      this.isReg = true;
    },
    //确定
    addUser: function() {
      if (this.name === "" || this.password === "" || this.repeat === "") {
        alert("请输入用户名和密码");
      } else if (this.checkUser) {
        alert("用户已存在");
      } else if (this.password !== this.repeat) {
        alert("两次输入密码不一致");
      } else {
        store.commit("addUser", {
          name: this.name,
          password: this.password
        });
        localStorage.setItem("users", JSON.stringify(store.state.users)); //存到本地
        this.name = "";
        this.password = "";
        this.repeat = "";
        this.isReg = false;
        alert("注册成功");
      }
    },
    //取消
    cancel: function() {
      this.isReg = false;
      // //axios请求
      // this.$axios.get()
      // .then(res=>{
      //   console.log(res)
      // })
      // .catch(err=>{
      //   console.log(err)
      // })
    }
  }
};
</script>

<style scoped lang="scss">
.header {
  margin: 25px;
  margin-left: 45px;
  font-weight: bold;
}

.form {
  display: flex;
  justify-content: flex-end;
  margin: 10px;
  margin-right: 15px;

  label {
    height: 30px;
    line-height: 30px;
    margin-right: 10px;
  }

  input {
    width: 75%;
    height: 30px;
    line-height: 30px;
    border-radius: 7px;
    border: 1 solid #d5d5d5;
    padding: 0 10px;
    box-sizing: border-box;
  }
}

.buttons {
  display: flex;
  margin: 30px 20px;

  button {
    flex: 1;
    margin: 5px;
    height: 35px;
    line-height: 35px;
    border-radius: 7px;
    border: 1px solid #42b983;
    cursor: pointer;

    &.first {
      background: #42b983;
      color: #fff;
    }

    &.second {
      color: #42b983;
    }
  }
}
</style>
