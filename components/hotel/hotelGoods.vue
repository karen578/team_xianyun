<template  v-for="(item,index) in goodsForm" :key="index" >
  <div class="hotelGoods">
    <el-col>
      <el-row type="flex" justify="space-around">
        <el-col :span="8" class="hotelPic">
          <img :src="goodslist.photos" alt />
        </el-col>
        <el-col :span="12">
          <div class="productHotel">
            <h4 @click="toDatail">{{goodslist.name}}</h4>
            <p>
              <span>{{goodslist.alias}}</span>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <span>{{goodslist.hoteltype.name}}</span>
            </p>
            <div class="pingjia">
              <i class="el-rate__icon el-icon-star-on"></i>
              <i class="el-rate__icon el-icon-star-on"></i>
              <i class="el-rate__icon el-icon-star-on"></i>
              <i class="el-rate__icon el-icon-star-on"></i>
              <i class="el-rate__icon el-icon-star-on"></i>
              <span>
                <strong>27</strong>条评价
              </span>
              <span>
                <strong>61</strong>篇游记
              </span>
            </div>
            <div class="locations">
              <i class="iconfont iconlocation" style="color:#666"></i>
              {{goodslist.address}}
            </div>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="otherTrips" v-for="(item,index) in goodslist.products" :key="index">
            <a href="javasprict:;">
              <el-row type="flex" justify="space-around">
                <span>{{item.name}}</span>
                <span>
                  <strong>￥{{item.price}}</strong>起 &gt;
                </span>
              </el-row>
            </a>
          </div>
        </el-col>
      </el-row>
    </el-col>
  </div>
</template>
<script>
export default {
  props: {
    goodslist: {
      type: Object,
      default: {
        hoteltype: {},
        hotellevel: {},
        products: []
      }
    }
  },
  data() {
    return {};
  },
  methods: {
    toDatail() {
      this.$router.push({
        path: `/hotel/detail?id=${this.goodslist.id}`
      });
    }
  }

  // mounted(){
  //   this.$axios({
  //     url: "/hotels",
  //     params: {
  //       city: this.$route.query.city
  //     }
  //   }).then(res => {
  //     this.goodsForm=res.data.data
  //   });
  // }
};
</script>
<style lang="less" scoped>
.hotelGoods {
  height: 245px;
  margin: 20px 0;
  border-bottom: 1px solid #ccc;
  .hotelPic {
    img {
      width: 320px;
      height: 210px;
    }
  }
  .productHotel {
    margin-left: 20px;
    h4 {
      font-size: 24px;
      font-weight: normal;
      cursor: pointer;
    }
    p {
      color: #666;
      margin-bottom: 15px;
    }
    .pingjia {
      margin-bottom: 15px;
      strong {
        color: #ffa600;
        font-weight: normal;
      }
      span {
        margin-right: 100px;
      }
    }
    .locations {
      font-size: 14px;
      color: #666;
    }
    i {
      color: rgb(247, 186, 42);
    }
  }
  .otherTrips {
    a {
      display: block;
      border-bottom: 1px solid #eee;
      padding: 15px 0;
      color: #666;
      cursor: pointer;
      font-size: 14px;
      strong {
        color: #ffa600;
        font-weight: normal;
      }
    }
  }
}
</style>


