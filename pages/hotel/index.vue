<template>
  <div class="hotel">
    <div class="top">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item>酒店</el-breadcrumb-item>
        <el-breadcrumb-item>{{cityName}}酒店预订</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="search">
      <el-row type="flex">
        <!-- 地点下拉框 -->
        <el-autocomplete
          v-model="cityName"
          :fetch-suggestions="querySearchAsync"
          placeholder="目的地"
          @select="handleSelectArr"
        ></el-autocomplete>
        <!-- 日期 -->
        <el-date-picker
          v-model="stayTime"
          type="daterange"
          range-separator="-"
          start-placeholder="入住日期"
          end-placeholder="离店日期"
          @change="SelectTime"
        ></el-date-picker>
        <!-- 人数 -->
        <el-input
          placeholder="人数未定"
          v-model="hotelFrom.person"
          class="input-with-select"
          readonly
          v-popover:popover4
        >
          <i class="el-input__icon iconfont iconuser" slot="suffix"></i>
        </el-input>
        <!-- 点击有框 -->
        <el-popover ref="popover4" placement="bottom" width="326" trigger="click" v-model="visible">
          <el-row type="flex" justify="space-around">
            <el-col :span="8">
              <p>每间</p>
            </el-col>
            <el-col :span="16">
              <el-row type="flex" justify="space-around">
                <el-select v-model="aults" placeholder="请选择" style="margin-right:5px" @change="selectAults">
                  <el-option
                    v-for="(item,index) in [1,2,3,4,5,6,7]"
                    :key="index"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
                <el-select v-model="childs" placeholder="请选择" @change="selectChilds">
                  <el-option
                    v-for="(item,index) in [0,1,2,3,4]"
                    :key="index"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
              </el-row>
            </el-col>
          </el-row>
          <div style="text-align: right; margin-top: 40px">
            <el-button type="primary" @click="submitPeople">确定</el-button>
          </div>
        </el-popover>
        <!-- 按钮 -->
        <el-button type="primary" @click="submitPrice">查看价格</el-button>
      </el-row>
    </div>
    <!-- 酒店文字 -->
    <LocationText :data="streets" />
    <!-- 酒店筛选 -->
    <HotelSearch @getPriceList="getPriceList"/>
    <!-- 酒店产品页面 -->
    <HotelGoods v-for="(item,index) in goodsForm" :key="index" :goodslist="item" />
    <!-- 分页 -->
    <HotelPage :dataForm="dataForm" @getPages="getPages" />
  </div>
</template>
<script>
import moment from "moment";
import LocationText from "@/components/hotel/locationText.vue";
import HotelSearch from "@/components/hotel/hotelSearch.vue";
import HotelGoods from "@/components/hotel/hotelGoods.vue";
import HotelPage from "@/components/hotel/hotelPage.vue";
export default {
  components: {
    HotelGoods,
    LocationText,
    HotelSearch,
    HotelPage
  },
  data() {
    return {
      visible: false,
      // 街道的集合
      streets: [],
      //城市的集合
      arr: [],
      cityName: "",
      hotelFrom: {
        id: 0,
        city: 0,
        price_in: 0, //酒店价格
        scenic: 0,
        name_contains: "",
        hotellevel: 2,
        hoteltype: 4,
        hotelbrand: 1,
        hotelasset: 1,
        enterTime: "", //入住时间
        leftTime: "", //入住时间
        person: 0, //人数
        _sort: "",
        _limit: 0,
        _start: 0,
        stayTime: "" //绑定日期的时间
      },
      state3: "",
      stayTime: "", //绑定的时间,
      // 成人的数据
      aults: "",
      childs: "",
      // 酒店集合
      goodsForm: [],
      //分页缓存数据
      dataForm: []
    };
  },
  methods: {
    //筛选价格获取回来的数据
    getPriceList(values){
     this.dataForm=values
     //调用分页方法
     this.getPages(this.dataForm);


    },
    //获取分页的数据
    getPages(data) {
      this.goodsForm = data;
    },
    //查看价格
    submitPrice() {
      console.log(123);
      this.$axios({
        url: "/hotels",
        params: {
          city: this.hotelFrom.city,
          enterTime: this.hotelFrom.enterTime,
          leftTime: this.hotelFrom.leftTime
        }
      }).then(res => {
        //请求成功了，等分页
        //需要调用分页数据
        this.dataForm = res.data.data;
        // 分页数据是要传入缓存的数据
        this.getPages(this.dataForm);
      });
    },
    //确定人数
    submitPeople() {
      this.hotelFrom.person = this.aults+ this.childs;
      this.visible = false;
    },
    //选中时间触发的事件
    SelectTime(date) {
      if (!date) {
        return;
      }
      this.hotelFrom.enterTime = moment(date[0]).format("YYYY-MM-DD");
      this.hotelFrom.leftTime = moment(date[1]).format("YYYY-MM-DD");
    },
    // 远程搜索选中触发的事件
    handleSelectArr(item) {
      console.log(item);
      this.hotelFrom.city = item.id;
      this.streets = item.scenics;
      //拼接路由
      this.$router.push(`/hotel?city=${item.id}`);
      this.$axios({
        url: "/hotels",
        params: {
          city: item.id
        }
      }).then(res => {
        //需要调用分页数据
        this.dataForm = res.data.data;
        // 分页数据是要传入缓存的数据
        this.getPages(this.dataForm);
      });
    },
    //远程搜索展示的数据，cb返回的是一个数组，数组里面是对象，有一个value属性
    querySearchAsync(value, cb) {
      if (!value) {
        //因为cb是回调函数，所以需要加上（）
        cb([]);
        return;
      }
      this.$axios({
        url: "/cities",
        params: {
          name: value
        }
      }).then(res => {
        this.arr = res.data.data;
        let newArr = this.arr.map(item => {
          item.value = item.name;
          return item;
        });
        this.cityName = newArr[0].value;
        cb(newArr);
      });
    },
    //选中成年人数触发事件
    selectAults(value) {
      this.aults=value+"成人"
    },
     selectChilds(value) {
      this.childs=value+"儿童"
    }
  },
  //可以监听所有属性
  
  mounted() {
    if (!this.hotelFrom.person) {
      this.hotelFrom.person = "";
    }
    const { city } = this.$route.query;
    this.hotelFrom.city = city;

    this.$axios({
      url: "/hotels",
      params: {
        city: this.$route.query.city
      }
    }).then(res => {
      console.log(this.dataForm);
      this.goodsForm = res.data.data;
      this.dataForm = [...res.data.data];
      this.cityName = this.goodsForm[0].city.name;
      this.$axios({
        url: "/cities",
        params: {
          name: this.cityName
        }
      }).then(res => {
        // 让页面的区域显示出来
        this.streets = res.data.data[0].scenics;
      });
    });
  }
};
</script>

<style lang="less" scoped>
.hotel {
  width: 1000px;
  margin: 0 auto;
  margin-top: 20px;
  /deep/.el-icon-arrow-right {
    margin-bottom: 20px;
  }
  .search {
    margin-bottom: 20px;
    /deep/.el-input {
      width: 220px;
      margin-left: 5px;
      margin-right: 20px;
    }
  }
}
</style>

