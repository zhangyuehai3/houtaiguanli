<template>
  <div>
       <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item><a href="/">权限管理</a></el-breadcrumb-item>
      <el-breadcrumb-item>权限列表
      </el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
        <div id="main" style="width:900px;height:400px"></div>
    </el-card>
  </div>
</template>

<script>
import echarts from 'echarts'
import _ from 'lodash'
export default {
data(){
  
},
created(){},
 async mounted(){
var myChart = echarts.init(document.getElementById('main'))
const {data:res} =await this.$http.get('reports/type/1')
if(res.meta.status !==200){
    return this.$message.error('获取折线图数据失败！')
}
const result =_.merge(res.data,this.options)
myChart.setOption(result)
},
methods:{

}
}
</script>

<style lang='less' scoped>

</style>