<template>
  <div>
            <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item><a href="/">权限管理</a></el-breadcrumb-item>
      <el-breadcrumb-item >权限列表 </el-breadcrumb-item>
    </el-breadcrumb>
    <el-card>
         <el-row>
             <el-col :span="8">
                 <el-input placeholder="请输入内容" >
       <el-button slot="append" icon="el-icon-search"></el-button>
  </el-input>
             </el-col>
         </el-row>
         <el-table :data="orderlist" border stripe>
            <el-table-column type="index"></el-table-column>
            <el-table-column label="订单编号" prop="order_number"></el-table-column>
            <el-table-column label="订单价格" prop="order_price"></el-table-column>
            <el-table-column label="是否付款" prop="pay_status">
                <template slot-scope="scope"> 
                <el-tag type="success" v-if="scope.row.pay_status==='1'">已付款</el-tag>
                <el-tag type="danger" v-else> 未付款</el-tag>
                </template>
            </el-table-column>
            <el-table-column label="是否发货" prop="is_send"></el-table-column>
            <el-table-column label="下单时间" prop="create_time">
                <template slot-scope="scope">
                    {{scope.row.create_time | dateFormat}}
                </template>
            </el-table-column>
            <el-table-column label="操作">
                <template slot-scope="scope">
                    <el-button size="mini" type="primary" icon="el-icon-edit"></el-button>
                    <el-button size="mini" type="success" icon="el-icon-location"></el-button>
                </template>
            </el-table-column>


         </el-table>
          <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="querInfo.pagenum"
      :page-sizes="[5,10,15]"
      :page-size="querInfo.pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>

    </el-card>
  </div>
</template>

<script>
export default {
data(){
    return{
querInfo:{
    query:'',
    pagenum:1,
    pagesize:10
},
tatol:0,
orderlist:[]
    }
},
created(){
    this.getOrderList()
},
methods:{
    async getOrderList(){
        const {data:res} = await this.$http.get('orders',{params: this.querInfo})
        if(res.meta.status !==200){
            return this.$message.error('获取订单列表失败！ ')
        }
        this.total =res.data.total
        this.orderlist =res.data.goods
    },
    handleSizeChange(newSize){
        this.querInfo.pagesize = newSize
        this.getOrderList()
    },
    handleCurrentChange(newPage){
        this.querInfo.pagenum =newPage
        this.getOrderList()
    }
}
}
</script>

<style lang='less' scoped>

</style>