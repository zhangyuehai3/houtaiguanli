<template>
  <div>
          <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item><a href="/">权限管理</a></el-breadcrumb-item>
      <el-breadcrumb-item>权限列表 </el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
       <el-alert title="添加商品信息" type="info" center show-icon :closable="false">

           </el-alert> 
           <el-steps :space="200" :active="activeIndex-0" finish-status="success" align-center="">
               <el-step title="基本信息"></el-step>
               <el-step title="商店参数"></el-step>
               <el-step title="商品属性"></el-step>
               <el-step title="商品照片"></el-step>
               <el-step title="商品内容"></el-step>
                <el-step title="完成"></el-step>
           </el-steps>
        <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="100px" class="demo-ruleForm" label-position="top">
           <el-tabs v-model="activeIndex" :tab-position="'left'" :before-leave='beforeTabLeave' @tab-click="tabClicked">
    <el-tab-pane label="基本信息" name="0">
   <el-form-item label="商品名称" prop="goods_name" >
       <el-input v-model="addForm.goods_name"></el-input>
   </el-form-item>
    </el-tab-pane>
   <el-form-item label="商品价格" prop="goods_price">
       <el-input v-model="addForm.goods_price" type="number"></el-input>
   </el-form-item>
   
   <el-form-item label="商品重量" prop="goods_weight" >
       <el-input v-model="addForm.goods_weight" type="number"></el-input>
   </el-form-item>
   
   <el-form-item label="商品数量" prop="goods_number" >
       <el-input v-model="addForm.goods_number" type="number"></el-input>
   </el-form-item>
   <el-form-item label="商品分类" prop="good_cat">
        <el-cascader
    v-model="addForm.goods_cat"
    :options="catelist"
    :props="cateProps"
    @change="handleChange"></el-cascader>
   </el-form-item>
   
   
    <el-tab-pane label="商品参数"  name="1">
        <el-form-item :label="item.attr_name" v-for="item in manyTableData" :key="item.attr_id">

        </el-form-item>
    </el-tab-pane>
       <el-tab-pane label="商品属性"  name="2">商品属性</el-tab-pane>
    <el-tab-pane label="商品图片"  name="3">商品图片</el-tab-pane>
    <el-tab-pane label="商品内容"  name="4">商品内容</el-tab-pane>
  </el-tabs>
  </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
    data(){
        return{
activeIndex:'0',
addForm:{
    goods_name:"",
    goods_price:0,
    goods_weight:0,
    goods_number:0,
    goods_cat:[]

},
catelist:[],
cateProps:{
    label:'cat_name',
    value:'cat_id',
    children:'children'
},
oldActiveName:'',
manyTableData:[],
addFormRules:{
    goods_name:[
        {required:true,message:'请输入商品名称',trigger:'blur'},
    ],
       goods_price:[
        {required:true,message:'请输入商品名称',trigger:'blur'},
    ],
  goods_weight:[
        {required:true,message:'请输入商品名称',trigger:'blur'},
    ],
  goods_number:[
        {required:true,message:'请输入商品名称',trigger:'blur'},
    ],
    good_cat:[
        {required:true,message:'请输入选择商品分类',trigger:'blur'}
    ]


    
}
    }

    },
    created(){
        this.getCateList()
    },
    methods:{
  async getCateList(){
      const {data :res} =await this.$http.get('categories')
      if(res.meta.status !==200){
          return this.$message.error('获取商店分类数据失败')
      }
      this.catelist = res.data
  },
  handleChange(){
      if(this.addForm.goods_cat.length !==3){
          this.addForm.good_cat =[]
      }
  },
  beforeTabLeave(activeName,oldActiveName){
      if(oldActiveName ==='0'&&this.addForm.goods_cat.length !==3){
          this.$message.error("请输入选择商品分类！")
          return false
      }
  },
  async tabClicked(){
if(this.activeIndex ==='1'){
    const{data:res} =await this.$http.get(`categories/${this.cateId}/attributes`,{params:{sel:'many'}})
    if(res.meta.status !==200){
        return this.$message.error('获取动态参数列表失败')
    }
    res.data.forEach(item=>{
        item.attr_vals = item.attr_vals.length ===0? []:item.attr_vals.split(' ')
    })
    this.manyTableData =res.data
}
  }
    },
computed:{
    cateId(){
        if(this.addForm.goods_cat.length===3){
            return this.addForm.goods_cat[2]
        }
        return null
    }
}

}
</script>

<style lang='less' scoped>

</style>