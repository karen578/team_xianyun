<template>
  <div class="Pagination">
    <!-- 当前页改变触发的事件@current-change -->
     <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagenum"
      :page-sizes="[1, 2, 3, 4]"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="dataForm.length">
    </el-pagination>
  </div>
</template>
<script>
export default {
  props:{
    dataForm:{
      type:Array,
      default:[]
    }
    
  },
  data() {
    return {
      pagenum: 1, //当前页
      pagesize: 2, //当前页面显示的条数
      total: 10, //总条数
      hotelListPage:[]
    };
  },
  methods: {
    //当前页面改变触发
    handleCurrentChange(val){
      // console.log(val);
      this.pagenum=val
      this.getPage()
      
    },
    handleSizeChange(val){
      //  console.log(val);
      this.pagesize=val
     this.getPage()
    },
    //分装一个分页数据
    getPage(){
     this.hotelListPage=this.dataForm.slice(
       (this.pagenum-1)*this.pagesize,
       (this.pagenum-1)*this.pagesize+this.pagesize
     )  
     //把分页的数据发给父组件
      this.$emit("getPages",this.hotelListPage)
    }
  }
};
</script>
<style lang="less" scoped>
.Pagination{
  height: 40px;
  line-height: 40px;
  margin: 20px 0;
  margin-left: 450px;
  
}
</style>


