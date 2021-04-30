<template>
  <div class="Login-conter">
    <div class="Login-box">
      <div class="avater-box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <el-form ref="loginFormReset" :model="loginForm" :rules="loginFormRules" class="login-form">
        <el-form-item prop="username">
          <el-input
            prefix-icon="el-icon-search"
            v-model="loginForm.username" 
          ></el-input>
        </el-form-item>

        <el-form-item prop="password">
          <el-input
            prefix-icon="el-icon-search"
            v-model="loginForm.password"
          ></el-input>
        </el-form-item>

        <el-form-item class="btns">
          <el-button type="primary" @click="Login">登入</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      loginForm:{
        username:'admin',
        password:'123456'
      },
      loginFormRules:{
          passname: [
            { required: true, message: '请输入活动名称', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          password: [
           { required: true, message: '请输入活动名称', trigger: 'blur' },
            { min: 3, max: 6, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
  
}




  }},
methods: {
  resetLoginForm(){
    this.$refs.loginFormReset.resetFields();
  },
  Login(){
    this.$refs.loginFormReset.validate(async valid=>{ if(!valid) return;
    const {data:res} =await this.$http.post("login", this.loginForm);
    if(res.meta.status !==200) return this.$message.error("登入失败");
    this.$message.success("登入成功");
    window.sessionStorage.setItem("token",res.data.token);
    this.$router.push("/home");

    })
  }
}
}
</script>

<style lang='less' scoped>
.Login-conter {
  background-color: rgb(0, 55, 255);
  height: 100%;
}
.Login-box {
  width: 450px;
  height: 300px;
  background-color: azure;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.avater-box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #eee;
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
.login-form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns {
  display: flex;
  justify-self: flex-end;
}
</style>