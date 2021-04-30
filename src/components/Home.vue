<template>
  <el-container class="home-container">
    <el-header
      ><div>
        <img src="../assets/css/heima.jpg" alt="" /> <span>我是Warm-boy </span>
      </div>
      <el-button type="info" @click="logout">退出</el-button></el-header
    >
    <el-container>
      
      <el-aside width="isCollapse ? '64px:'200px'">
         <div class="toggle-button" @click="toggleCollapse">|||  </div>
        <el-menu
          default-active="2"
          class="el-menu-vertical-demo"
         
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409BFF"
          unique-opened
          :collapse='isCollapse'
          :collapse-transition ='false'
          router
          :defaultActive=" activePath"
        >
         
          <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
            <template slot="title">
              <i class="el-icon-user-solid"></i>
              <span>{{item.authName}}</span>
            </template>
             <el-menu-item :index="'/'+subItem.path " v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/'+subItem.path)">
               <template slot="title">
              <i class="el-icon-menu"></i>
              <span>{{subItem.authName}}</span>
            </template>
             </el-menu-item>
            
          </el-submenu>

        </el-menu></el-aside
        >
      <el-main><router-view></router-view></el-main>
    </el-container>
    
  </el-container>
</template>

<script>
export default {
  data () {
    return {
    menuList:[],
    menuObj:{
      '102':'el-icon-user-solid',
      '103':'el-icon-user-solid',
      '101':'el-icon-user-solid',
      '125':'el-icon-user-solid',
      '125':'el-icon-user-solid'
    },
    isCollapse:false,
    activePath:''
    }
  },
  created(){
this.getMenuList()
this.activePath =window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    async getMenuList(){
      const {data:res} =await this.$http.get('menus')
      if ( res.meta.status !==200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
    },
    toggleCollapse(){
this.isCollapse =! this.isCollapse
    },
    saveNavState(activePath){
      window.sessionStorage.setItem('activePath',activePath)
      this.activePath =activePath
    }
  },
};
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}
.el-aside {
  background-color: #333744;
  .el-menu{
    border-right: none;
  }
}
.el-main {
  background-color: #eaedf1;
}
div > img {
  height: 50px;
  width: 50px;
}
.iconfont{
  margin-right: 10px;
}
.toggle-button{
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
