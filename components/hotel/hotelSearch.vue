<template>
  <div class="selectNum">
    <el-row type="flex" justify="space-around">
      <el-col :span="8" class="tips">
        <el-row type="flex" justify="space-between">
          <p>价格</p>
          <p>0-{{price}}</p>
        </el-row>
        <el-slider v-model="price" :max="2000" @change="ChangePrice"></el-slider>
      </el-col>
      <el-col :span="6" class="hotelLevel">
        <p>住宿等级</p>
        <el-dropdown :hide-on-click="false">
          <p class="el-dropdown-link">
            <span>{{levelText||"不限"}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </p>
          <!-- 用v-model="closedropdown"绑定数据是为了让下拉菜单选中之后关闭 -->
          <el-dropdown-menu slot="dropdown" v-model="closedropdown">
            <el-dropdown-item
              v-for="(item,index) in hotellevels"
              :key="index"
              @click.native="getLevels(item)"
            >
              <i class="iconfont iconcircle"></i>
              {{item.name}}
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="6" class="hotelLevel">
        <p>住宿类型</p>
        <el-dropdown :hide-on-click="false">
          <p class="el-dropdown-link">
            <span>{{typesText||"不限"}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </p>
          <el-dropdown-menu slot="dropdown" v-model="closeType">
            <el-dropdown-item
              v-for="(item,index) in hoteltypes"
              :key="index"
              @click.native="getTypes(item)"
            >
              <i class="iconfont iconcircle"></i>
              {{item.name}}
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="6" class="hotelLevel">
        <p>酒店设施</p>
        <el-dropdown :hide-on-click="false">
          <p class="el-dropdown-link">
            <span>{{assetsText||"不限"}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </p>
          <el-dropdown-menu slot="dropdown" v-model="closeAssets">
            <el-dropdown-item
              v-for="(item,index) in hotelassets"
              :key="index"
              @click.native="getAssets(item)"
            >
              <i class="iconfont iconcircle"></i>
              {{item.name}}
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="6" class="hotelLevel">
        <p>酒店品牌</p>
        <el-dropdown :hide-on-click="false">
          <p class="el-dropdown-link" >
            <span>{{brandsText||"不限"}}</span>
            <i class="el-icon-arrow-down el-icon--right"></i>
          </p>
          <el-dropdown-menu slot="dropdown" v-model="closebrands" :style="{height:'300px',overflow:'auto'}">
            <el-dropdown-item
              v-for="(item,index) in hotelbrands"
              :key="index"
              @click.native="getbrands(item)"
            >
              <i class="iconfont iconcircle"></i>
              {{item.name}}
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      closebrands: false,
      closeAssets: false,
      closeType: false,
      closedropdown: false,
      //酒店等级集合
      hotellevels: [],
      //住宿类型集合
      hoteltypes: [],
      //酒店品牌集合
      hotelbrands: [],
      //酒店设施集合
      hotelassets: [],
      price: 0,
      priceList: [],
      //选中星级显示的数据
      levelText: "",
      //选中住宿等级显示的数据
      typesText: "",
      //选中酒店设施显示的数据
      brandsText: "",
      assetsText: "",
      levelList: [],
      typesList: [],
      assetsList: [],
      brandsList: []
    };
  },
  methods: {
    //选中品牌触发的事件
    getbrands(item) {
      console.log(item);
      this.brandsText = item.name;
      const brands = item.id;
      this.closebrands = false;
      this.$axios({
        url: "hotels",
        params: {
          city: this.$route.query.city,
          hotelbrand: brands
        }
      }).then(res => {
        // console.log(res);
        this.brandsList = res.data.data;
        this.$emit("getPriceList", this.brandsList);
      });
    },
    //选中酒店设施触发的事件
    getAssets(item) {
      console.log(item);
      this.assetsText = item.name;
      const assets = item.id;
      this.closeAssets = false;
      this.$axios({
        url: "hotels",
        params: {
          city: this.$route.query.city,
          hotelasset: assets
        }
      }).then(res => {
        // console.log(res);
        this.assetsList = res.data.data;
        this.$emit("getPriceList", this.assetsList);
      });
    },
    //选中酒店类型触发的事件
    getTypes(item) {
      // console.log(item);
      this.typesText = item.name;
      const type = item.id;
      this.closeType = false;
      this.$axios({
        url: "hotels",
        params: {
          city: this.$route.query.city,
          hoteltype: type
        }
      }).then(res => {
        this.typesList = res.data.data;
        this.$emit("getPriceList", this.typesList);
      });
    },
    //选中酒店等级触发事件
    getLevels(item) {
      this.levelText = item.name;
      const level = item.level;
      this.closedropdown = false;
      this.$axios({
        url: "hotels",
        params: {
          city: this.$route.query.city,
          hotellevel: level
        }
      }).then(res => {
        this.levelList = res.data.data;
        this.$emit("getPriceList", this.levelList);
      });
    },
    //改变价格触发的事件
    ChangePrice(value) {
      this.price <= value;
      console.log(value);
      this.$axios({
        url: "/hotels",
        params: {
          city: this.$route.query.city,
          price_lt: this.price
        }
      }).then(res => {
        this.priceList = res.data.data;
        this.$emit("getPriceList", this.priceList);
      });
    }
  },
  mounted() {
    this.$axios({
      url: "/hotels/options"
    }).then(res => {
      // console.log(res);
      const { levels, assets, brands, types } = res.data.data;
      this.hotellevels = levels;
      this.hoteltypes = types;
      this.hotelbrands = brands;
      this.hotelassets = assets;
    });
  }
};
</script>
<style lang="less" scoped>
.selectNum {
  color: #666;
  margin-top: 20px;
  border: 1px solid #ccc;
  margin-bottom: 30px;
  /deep/.el-dropdown {
    outline: none;
  }
  .tips {
    padding: 15px 20px;
  }
  .hotelLevel {
    border-left: 1px solid #ccc;
    margin: 5px 0;
    padding: 0 20px;
    p {
      margin-top: 10px;
      span {
        margin-right: 80px;
      }
    }
  }
}
</style>

