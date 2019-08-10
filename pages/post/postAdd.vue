<template>
  <div class="container">
    <div class="gongnue">
      <div class="fenxiang">
        <el-row class="biaoti">
          <h3>发表新攻略</h3>
          <i>分享你的个人游记，让更多人看到哦！</i>
        </el-row>
      </div>
    </div>
    <div class="search-bar">
      <el-input v-model="form.username" placeholder="请输入标题"></el-input>
    </div>
    <!-- 富文本框的盒子 -->
    <div class="RichTextBox" v-quill :content="form.content">
      <!-- <el-form v-model="form.neiRong" > </el-form> -->
    </div>
    <!-- 搜索的盒子 -->
    <div class="search-form">
      <el-form ref="form" label-width="80px">
        <el-form-item label="选择城市">
          <el-autocomplete
            :fetch-suggestions="queryDepartSearch"
            placeholder="请搜索游玩城市"
            @select="handleDepartSelect"
            v-model="form.playCity"
            class="el-autocomplete"
          ></el-autocomplete>
        </el-form-item>
      </el-form>
    </div>
    <div class="shousuo">
      <el-button class="submit" type="primary" @click="handleRegSubmit">
        <p>发布</p>
      </el-button>或者
      <span class="anniu" type @click="savetoDraft">保存到草稿</span>
    </div>
    <!-- 侧边栏 -->
    <div class="aside">
      <!-- 侧边栏组件 -->
      <Drafts />
    </div>
  </div>
</template>
  
<script>
//引入样式
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
// 引入组件

import Drafts from "@/components/post/Drafts.vue";


export default {
  data() {
    return {
      //  input: '',
      form: {
        playCity: "", //游玩城市
        playCode: "", //游玩城市的代码
        username: "", //输入框
        content: ""   //富文本框
      }
    };
  },
   //使用watch监听所有属性得变化。
        watch:{
            //监听路由信息的变化
            $route(){
                //输出一下看看监听是否成功
                // console.log(this.$route)
            }
        },

  methods: {
    // 出发城市每次输入值时候触发
    //value：输入框的值
    //cd： 回调函数，必须要调用，接受的参数格式是固定的,必须是是一个数组，数组中每一项必须是一个对象
    //对象中必须包含value属性
    queryDepartSearch(value, cb) {
      //每次在输入框输入内容都会触发（queryDepartSearch）这个函数，然后这个函数会根据value这个值向后台发出请求

      //判断如果没有value输入框这个值的话，就不会往下执行代码
      if (!value) {
        cb([]); //调用cb() 传参数 如果参数是空的，没有值。列表自然就不会出现选择框 （所以传个空的数组进去）
        return; //阻止往下执行
      }

      //向后台发出请求获取城市列表
      this.$axios({
        url: "/airs/city",
        //设置get请求的参数
        params: {
          name: value
        }
      }).then(res => {
        // console.log(res.data);   //打印输出查看一下返回来的数据
        //拿到返回的数据 存到{data}
        const { data } = res.data;
        //循环给每一项数据添加一个value属性

        //先定义一个空的数组
        const newData = [];
        //用forEach循环把数据的每一项属性找到。 < v 就是代表数据里面的每一个属性 >
        data.forEach(v => {
          //给每一项都加了 value值，并且找到每一项name值 使用replace()方法 查找'市'出来换成空的。
          v.value = v.name.replace("市", "");
          //用push方法向数组的末尾添加一个v元素，也就是value值
          newData.push(v);
        });
        //调用CB（） 把做好效果的新创建列表传进去
        cb(newData);
      });
    },
    // 选择城市下拉选择时触发
    handleDepartSelect(item) {
      // console.log(item)   打印输出查看 item是一个对象，里面有个参数叫sort 是城市代码
      this.form.playCity = item.value;
      //拿到item里面的sort 赋值给表单里面的departCode
      this.form.playCode = item.sort;
    },
    // 提交表单是触发
    handleRegSubmit() {
      if (!this.form.username) {
        this.$alert("标题不能为空", "提示", {
          type: "warning"
        });
        return;
      }
      //调用axios向后台发送请求
      this.$axios({
        url: "/posts",
        //请求的方法：post
        method: "POST",
        headers: {
          Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
        },
        data: {
          //拿到表单里面的内容
          content: this.form.content,
          title: this.form.username,
          city: this.form.playCity
        }
      }).then(res => {
        this.$alert("恭喜你,文章新增成功", {
          type: "warning"
        });
      });
    },
    savetoDraft() {
      // 获取到本地的（vuex）的数据
      //  const arr=[...this.$store.state.air.history];
      const arr = JSON.parse(JSON.stringify(this.$store.state.air.history));
      //  console.log(arr)
      //新的记录添加到第一个
      arr.unshift(this.form);
      if (arr.length > 5) {
        arr.length = 5;
      }
      let newArr = JSON.parse(JSON.stringify(arr));

      //调用vuex的方法保存数据
      this.$store.commit("air/setHistory", newArr);
    }
  },
  // 进行组件注册
  components: {
    // VueQuillEditor,
    Drafts
  }
};
</script>

<style scoped lang="less">
.container {
  width: 1000px;
  height: 668px;
  margin: 0 auto;
  position: relative;
  .gongnue {
    width: 1000px;
    padding-top: 20px;
    font-family: "微软雅黑";
    // margin-left: 66px;
  }
  .biaoti {
    .h3 {
      font-size: 10px;
      color: #101566;
    }
    i {
      line-height: 50px;
      color: #a4b2cc;
    }
  }
  .search-bar {
    width: 800px;
    // margin-left: 66px;
    margin-bottom: 20px;
  }
  .RichTextBox {
    height: 300px;
    border-bottom: 1px solid #ccc;
    // overflow: auto;
    width: 1000px;
  }
  .search-form {
    padding-top: 20px;
  }
  .shousuo {
    margin-bottom: 20px;
    .submit {
      width: 6%;
      height: 8%;
      padding-left: 13px;
    }
  }
  .aside {
    position: relative; /*固定位置*/
    z-index: 99; /*设置优先级显示，保证不会被覆盖*/
    right: -409px;
    bottom: 326px;
    width: 250px;
  }
  .anniu {
    color: rgba(255, 196, 0, 0.774);
    border: none;
    cursor: pointer; 
 
  }
  /deep/ .ql-snow {
    width: 800px;
  }
 
}
</style>
