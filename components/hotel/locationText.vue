<template>
  <div class="locationText">
    <el-row type="flex" class="row-bg" justify="space-between">
      <el-col :span="14">
        <div class="contain">
          <el-row type="flex" class="containText" justify="space-between">
            <el-col :span="3">
              <p>区域：</p>
            </el-col>
            <el-col :span="21">
              <a href="#">全部</a>
              <a href="#" v-for="(item,index) in data" :key="index">{{item.name}}</a>
              <span><i class="el-icon-d-arrow-right"></i> 等{{data.length}}个区域</span>
            </el-col>
          </el-row>
          <el-row type="flex" class="containText" justify="space-between">
            <el-col :span="3">
              <p>攻略：</p>
            </el-col>
            <el-col :span="21">
              <p>北京，你想要的都能在这找到。博古通今，兼容并蓄，天下一城，如是帝都。 景点以故宫为中心向四处辐射；地铁便宜通畅，而且覆盖绝大多数景点。 由于早上有天安门升旗仪式，所以大多数人选择在天安门附近住宿。</p>
            </el-col>
          </el-row>
          <el-row type="flex" class="containText" justify="space-between">
            <el-col :span="3">
              <p>均价：</p>
            </el-col>
            <el-col :span="21">
              <el-row type="flex">
                <div class="firstStar">
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <span>￥332</span>
                </div>
                <div class="firstStar">
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <span>￥521</span>
                </div>
                <div class="firstStar">
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <span>￥768</span>
                </div>
              </el-row>
            </el-col>
          </el-row>
        </div>
      </el-col>
      <el-col :span="10">
        <div id="container"></div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  props:{
    // 父主件传递过来的数据
    data:{
      type:Array,
      default:[]
    }
  },
  mounted() {
    window.onLoad = function() {
      var map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [118.8718107, 31.32846821], //中心点坐标
        viewMode: "3D" //使用3D视图
      });

      // 创建一个 Marker 实例：
      var marker = new AMap.Marker({
        position: new AMap.LngLat(118.8718107, 31.32846821), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
        title: "北京"
      });

      // 将创建的点标记添加到已有的地图实例：
      map.add(marker);
    };
    var url =
      "https://webapi.amap.com/maps?v=1.4.15&key=6e84858af4fa6472a6bdbd646004b471&callback=onLoad";
    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  }
};
</script>
<style lang="less" scoped>
.locationText {
    .contain{
         color: #666;
          font-size: 14px;
          margin-right: 15px;
        .containText{
            a{
                margin-right: 5px;
            }
           
            /deep/.el-col{
                margin: 10px 0;
                
            }
            i{
                color: #ff9900;
            }
            .firstStar{
                margin-right: 30px;
            }
        }

    }



  #container {
    width: 420px;
    height: 260px;
  }
}
</style>


