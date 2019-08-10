<template>
    <div class="aside">
        <div class="history">
            <h5>草稿箱</h5>
            <nuxt-link :to="`/post?username=${item.username}&content=${item.content}&playCity=${item.playCity}`"
            v-for="(item,index) in $store.state.air.history"
                :key='index'>
                <el-row type="flex" 
                justify="space-between" 
                align="middle"
                class="history-item"
                >
                    <div class="air-info">
                        <i class="el-icon-edit"></i>
                        <div class="to-from">{{item.username}}</div>
                        <p>2019-08-10</p>
                    </div>
                    <!-- <span>选择</span> -->
                </el-row>
            </nuxt-link>
        </div>
    </div>
</template>

<script>
export default {
       
}
</script>

<style scoped lang="less">

.history{
    border:1px #ddd solid;
    padding:10px;
    margin-top:10px;

    h5{
        font-size: 16px;
        font-weight: normal;
        padding-bottom: 10px;
        border-bottom:1px #eee solid;
    }

    .history-item{
        padding:10px 0;
        font-size: 14px;
        border-bottom: 1px #eee solid;

        &:last-child{
            border:none;
        }

        .to-from{
            margin-bottom: 5px;
        }

        p{
            font-size: 12px;
            color:#666;
        }

        span{
            color:#fff;
            display: block;
            padding:2px 10px;
            background: orange;
            border-radius: 4px;
            font-size:12px;
            cursor: pointer;
        }
        
    }
}
</style>