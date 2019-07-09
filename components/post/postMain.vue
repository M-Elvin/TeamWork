<template>
    <div class="post-wrapper">
        <div class="search-wrapper">
            <div class="search-bar el-row is-justify-space-between is-align-middle el-row--flex">
                <input type="text" placeholder="请输入想去的地方" v-model="search" @select="handlePostSelect">
                <i class="el-icon-search" @click="handleSubmit"></i>
            </div>
            <div class="search-recommend">
                推荐：
                <span>广州</span>
                <span>上海</span>
                <span>北京</span>
            </div>
        </div>
        <div class="post-title el-row is-justify-space-between is-align-middle el-row--flex">
            <h4>推荐攻略</h4>
            <button type="button" class="el-button el-button--primary">
                <i class="el-icon-edit"></i>
                <span @click='handleTiao'>写游记</span>
            </button>
        </div>
        <!-- 渲染数据 -->
        <ul class="post-list">
            <li  v-for="(item,index) in dataList" :key="index" >
                <div class="post-item card" v-if="item.images.length >= 3">
                        <!-- 标题 -->
                    <h4 class="post-title" >
                        <a href="#">{{item.title}}</a>
                    </h4>
                    <!-- 详细 -->
                    <p class="post-desc">
                        <a href="" v-html="item.summary">
                            {{item.summary}}
                        </a>
                    </p>
                    <!-- 图片 -->
                    <div class="card-images" >
                        <img :src="imgs" v-for="(imgs,index1) in item.images" :key="index1" >
                    </div>
                    <!-- 左图标 -->
                    <div class="post-info el-row is-justify-space-between el-row--flex">
                        <div class="post-info-left el-row is-align-middle el-row--flex">
                            <span class="el-icon-location-outline">{{item.cityName}}</span>
                            <div class="post-user el-row is-align-middle el-row--flex">
                                by
                                <a href="">
                                    <img src="http://157.122.54.189:9095/assets/images/avatar.jpg" alt="">
                                </a>
                                <a href="">{{item.account.nickname}}</a>
                            </div>
                            <span><i class="el-icon-view">&nbsp;{{item.watch}}</i></span>
                        </div>
                    </div>
                </div>
                <!-- 第二种样式 -->
                <div class="post-item image-text el-row is-justify-space-between  el-row--flex" v-else>
                    <!-- 图片 -->
                    <div class="post-cover el-row is-align-middle" >
                        <img :src="imgs" v-for="(imgs,index1) in item.images" :key="index1" >
                    </div>
                    <div class="post-content">
                        <!-- 标题 -->
                        <h4 class="post-title" >
                            <a href="#">{{item.title}}</a>
                        </h4>
                        <!-- 详细 -->
                        <p class="post-desc">
                            <a href="" v-html="item.summary">
                                {{item.summary}}
                            </a>
                        </p>
                        <!-- 左图标 -->
                    <div class="post-info el-row is-justify-space-between el-row--flex">
                        <div class="post-info-left el-row is-align-middle el-row--flex">
                            <span class="el-icon-location-outline">{{item.cityName}}</span>
                            <div class="post-user el-row is-align-middle el-row--flex">
                                by
                                <a href="">
                                    <img src="http://157.122.54.189:9095/assets/images/avatar.jpg" alt="">
                                </a>
                                <a href="">{{item.account.nickname}}</a>
                            </div>
                            <span><i class="el-icon-view">&nbsp;{{item.watch}}</i></span>
                        </div>
                    </div>
                </div>
                    </div>
                    
            </li>
           
        </ul>
        <div class="el-row is-justify-center el-row--flex" style="margin-top: 10px;">
            <div class="el-pagination">
                <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pageIndex"
                :page-sizes="[5, 10, 15, 20]"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="6">
                </el-pagination>
            </div>
        </div>

    </div>
</template>

<script>
export default {
     data() {
      return {
        search:"",
        dataList:[],//分页后数据
        dataMain:[],//拿到数据
        postData:{},
       
        pageIndex: 1, // 当前页数
        pageSize: 3,  // 显示条数
        total:0,
      };
    },
    mounted(){
        
        this.getData();
        
        
    },
    watch: {
        $route(){
            
            this.getData();
        }
    },
    
    methods: {
        
        handlePostSelect(item){
            this.search = item.value

        },
        handleSubmit(){
            
        },
        handleTiao(){
            this.$router.push('/post/create')
        },
        
        // 分页切换条数触发
        handleSizeChange(value){
            this.pageSize = value;
        },

        // 切换页数时触发
        handleCurrentChange(value){
            this.pageIndex = value;
            this.setDataList();
            
        },
        setDataList(arr){
            
            
            this.dataList =this.dataMain.slice(
                 (this.pageIndex - 1) * this.pageSize,
                this.pageIndex * this.pageSize
             );
            console.log(this.dataList);
            

        },
        getData(){
            this.$axios({
            url: `posts`,
            
            }).then(res => {
                
                this.total = res.data.total;
                // 初始化数据
                this.pageIndex = 1;
                this.dataList = res.data.data.slice(0,3);
                this.dataMain = res.data.data
               
        });
        }
    
    },
   
  

}
</script>

<style scoped lang="less">
.post-wrapper{
    width: 700px;
    .search-bar{
        width: 100%;
        box-sizing: border-box;
        height: 40px;
        line-height: 40px;
        border: 3px solid orange;
        input{
            flex: 1;
            padding: 0 20px;
            line-height: 40px;
            outline: none;
            border: none;
            background: none;
            
        }
        .el-icon-search{
            font-size: 24px;
            color: orange;
            font-weight: 700;
            margin-right: 10px;
        }
    }
    .search-recommend{
        padding: 10px 0;
        font-size: 12px;
        color: #666;
        span{
            margin-right: 5px;
        }
    }
    > .post-title{
        padding-bottom: 10px;
        border-bottom: 1px solid #eee;
        position: relative;
        h4{
            font-weight: 400;
            font-size: 18px;
            color: orange;
            &:after{
                display: block;
                content: "";
                width: 72px;
                height: 2px;
                background: orange;
                position: absolute;
                bottom: 0;
                left: 0;
            }
        }
        button{
            color: #fff;
            background-color: #409eff;
            border-color: #409eff;
            span{
                margin-left: 5px;
            }
        }
    }
    .post-list{
        .post-item {
            width: 100%;
            padding: 20px 0;
            border-bottom: 1px solid #eee;
            .post-content{
                width: 470px;
            }
            
            .post-title{
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                margin-bottom: 15px;
                font-weight: 400;
                font-size: 18px;
            }
        }
        .post-desc{
            margin-bottom: 15px;
            line-height: 1.5;
            font-size: 14px;
            height: 63px;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 3;
            a{
                color: #666;
            }
        }
        .post-cover{
            width: 220px;
            height: 150px;
            overflow: hidden;
            flex-shrink: 0;
            margin-right: 10px;
            img{
                display: block;
                width: 100%;
                height: 100%;
                -o-object-fit: cover;
                object-fit: cover;
            }
        }
        .card-images{
            overflow: hidden;
            margin: 15px 0;
            width: 720px;
            height: 150px;
            img{
                float: left;
                margin-right:20px;
                width: 220px;
                height: 150px;
                display: block;
                -o-object-fit: cover;
                object-fit: cover;
            }

        }
        .post-info{
            .post-info-left{
                font-size: 12px;
                color: #999;
                span{
                    margin-right: 10px;
                }
                .post-user{
                    margin-right: 10px;
                    a{
                        color: orange;
                        img{
                            display: block;
                            width: 16px;
                            height: 16px;
                            border-radius: 100px;
                            margin: 5px;
                        }
                    }

                }
            }
        }
    }
    .el-pagination{
        white-space: nowrap;
        padding: 2px 5px;
        color: #303133;
        font-weight: 700;
    }
}
</style>
