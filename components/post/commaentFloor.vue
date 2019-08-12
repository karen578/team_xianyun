<template>
  <div class="cmt-floor">
    <post-comment-floor
      v-if="comment.parent"
      :comment="comment.parent"
      @preview="handlePreview"
      @handleReply="handleReply"
    ></post-comment-floor>
    <div class="cmt-info">
      {{comment.account.nickname}}
      <i>{{comment.created_at | newTime}}</i>
      <span style="float: right">{{comment.level}}</span>
    </div>
    <!-- 评论楼层 -->
    <div class="cmt-content">
      <div class="cmt-new" style="position: relative">
        <p class="cmt-message">{{comment.content}}</p>
        <el-row type="flex">
          <div
            class="cmt-pic"
            v-for="(pic, picIndex) in comment.pics"
            :key="picIndex"
            @click="handlePictureCardPreview(pic)"
          >
            <img :src="$axios.defaults.baseURL + pic.url" />
          </div>
        </el-row>
        <div class="cmt-ctrl">
          <a href="javascript:;" style="display:block" @click="handleReply">回复</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "post-comment-floor",

  props: {
    comment: {
      type: Object,
      default: {}
    }
  },
  filters: {
    newTime(value) {
      return moment(value).format("YYYY-MM-DD-hh");
    }
  },
  methods: {
    handlePreview(file) {
        console.log(file);
    },
    handlePictureCardPreview(file) {
        console.log(file)
        this.$emit('handlePictureCardPreview',file)
    },
    // 回复功能
    handleReply() {
      this.$emit("handleReply", this.comment);
    }
  },
  mounted() {}
};
</script>

<style lang="less" scoped>
.cmt-floor {
  border: 1px solid #dddddd;
  padding: 2px;
  margin-left: 4px;
  margin-bottom: 10px;
  background-color: #f9f9f9;
}
.cmt-info {
  margin-bottom: 10px;
  font-size: 12px;
  color: #666;
  padding: 0 0 0 10px;
}
.cmt-content {
  padding: 0 0 0 10px;
}
.cmt-message {
  margin-top: 10px;
  margin-bottom: 20px;
}
.cmt-pic {
  border: 1px dashed #ddd;
  padding: 4px;
  height: 70px;
  margin: 2px;
  img {
    width: 100px;
    height: 70px;
  }
}
.cmt-ctrl {
  float: right;
  font-size: 12px;
  color: blue;
  position: absolute;
  bottom: 0px;
  right: 0;
  a:hover{
                    text-decoration: underline;
                }
}
</style>

