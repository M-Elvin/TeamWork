


<template>
    <section class="contianer">
        <el-row  type="flex" justify="space-between">
            <el-row class="left" v-for='(item,index) in wenzhan' :key='index'>


                <!-- 攻略详情头部 -->
                <div class="detail-title">
                    <el-row  class="nav-title" type="flex" >
                        <nuxt-link to="/post">
                            旅游攻略
                        </nuxt-link>
                            <em>/</em>
                          <p>攻略详情</p>  
                    </el-row>
                        <h1>
                            {{item.title}}
                        </h1>
                    <el-row type="flex" justify="end" >
                        <span>攻略: 2019-05-22 10:57</span>
                        <span>阅读: 623</span>
                    </el-row>
                </div>


                <!-- 攻略详情内容部分 -->
                <el-row v-html="item.content" class="content-pic">
                  {{item.content}}
                </el-row>
                <el-row class="detail-content" type='flex' justify='center'>
                    <div >
                        <i class="icon el-icon-edit-outline"></i>
                        <p>评论(10)</p>
                    </div>
                    <div @click='handleCollect'>
                        <i class="icon el-icon-star-off"></i>
                        <p >收藏</p>
                    </div>
                    <div>
                        <i class="icon el-icon-share"></i>
                        <p>分享</p>
                    </div>
                    <div @click="handleDianZan">
                        <i class="icon el-icon-loading"></i>
                        <p>点赞({{1}})</p>
                    </div>
                </el-row>                
             



                <!-- 评论部分 -->
                <el-row class='comment'>
                    <p>评论</p>
                    <el-row class="reply"  v-if='false' type="flex" justify='space-between' align="middle">
                        <span>回复&nbsp;@</span>
                        <span class="detealx">x</span>
                    </el-row>
                    <input  
                    v-model="input" 
                    placeholder="说点什么吧..." />

                    <!-- 照片墙 -->
                  

                    <Comments/>
                    </el-row>
                </el-row>
                <!-- 侧边栏  相关攻略-->
                <Strategy/> 
        </el-row>
    </section>
</template>

<script>
import Strategy from '@/components/post/strategy.vue'
import Comments from '@/components/post/comments.vue'
export default {
    data(){
        return{
            input:'',
            dialogImageUrl: '',
            dialogVisible: false,
            // pageIndex: 1,//默认的页面 随着页面的切换而变化  
            // pageSize:5,//页面显示条数
            // total:0,//总条数
            wenzhan:[
                {title:''},
            ]
        }
        
    },
    components:{
        Strategy,
        Comments
    },
    mounted(){
        this.$axios({
            url: `/posts?id=6`,
            method:"GET",
            
        }).then(res=>{
            // console.log(res.data);
            // this.wenzhan=data;
            const {data} =res.data
            this.wenzhan=data;
        
            
        })
    },
     methods: {

    //   收藏文章
    handleCollect(){
        this.$axios({
            url:'/posts/star',
            method:"GET",
            params:
                this.$route.query.id
            ,
             headers: {
                    Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
                }
        }).then(res=>{
            console.log(res);
        })
    },
    //点赞
    handleDianZan(){
        this.$axios({
            url:'/posts/like',
            method:"GET",
            params:this.$route.query.id,
            headers: {
                    Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
                }
        }).then(res=>{
            console.log(res);
            this.$message.success("点赞成功")
        })
    }
    }
}
</script>

<style lang='less' scoped>
    .contianer{
        width: 1000px;
        margin: 0 auto;
        height: 100%;
       .left{
           width: 700px;
            .detail-title{
                
                .nav-title{
                    a{
                        color:#333;
                        font-weight: 600;
                        &:hover{
                            color:skyblue;
                        }
                    }
                    em{
                        color:#c4c4cc;
                        margin: 0 10px;
                    }
                    p{
                        color:#606266;
                    }
                    padding: 20px 10px 20px 0 ;
                }
                h1{
                    display: block;
                    font-size: 2em;
                    margin-block-start: 0.67em;
                    margin-block-end: 0.67em;
                    margin-inline-start: 0px;
                    margin-inline-end: 0px;
                    font-weight: bold;
                    border-bottom: 1px solid #ddd;
                    padding-bottom: 10px;
                }
                span{
                    color: #999;
                    padding: 0 10px;
                }
            
            }
            .content-pic{
               /deep/ img {
                 width: 100%;
               }
            }
            .detail-content{
                margin-top: 50px;
                >div{
                    text-align: center;
                    margin: 0 20px;
                    i{
                        display: block;
                        font-size: 28px;
                        color: orange;
                    }
                    p{
                        margin-top: 5px;
                        font-size: 14px;
                        color: #999;
                        
                    }
                }
                
            }
            .comment{
                p{
                    margin-bottom: 10px;
                }
                .reply{
                    width: 200px;
                    height: 32px;
                    line-height: 32px;
                    background: #f4f4f5;
                    border:1px solid #ccc;
                    border-radius: 10px;
                    color: #909399;
                    margin: 20px 0 10px 0;
                    padding: 0 10px;
                    // text-align: center;
                    .detealx{
                        width: 18px;
                        height: 18px;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        // text-align: center;
                        vertical-align: middle;
                        &:hover{
                            background: #909399;
                            color:#fff;
                            border-radius: 9px;
                        }
                    }
                }
                input{
                    width: 100%;
                    height: 54px;
                    text-indent: 2em;
                   margin-bottom: 15px;
                   border-radius: 5px;
                   border:1px solid #ccc;
                //    box-sizing: border-box;
                }
               
            }
        }
       .right{
           width: 280px;
       }
        
     
    }
</style>
