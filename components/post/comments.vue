<template>
    <div>
<!-- 照片墙 -->
          <el-row
            type="flex"
            justify="space-between"
            class="photo-wall">
            <el-upload
            
                action='http://127.0.0.1:1337/upload'
                list-type="picture-card"
                :on-preview="handlePictureCardPreview"
                :on-remove="handleRemove">
                <i class="el-icon-plus"></i>
            </el-upload>
            <el-dialog :visible.sync="dialogVisible">
                <img width="100%" :src="dialogImageUrl" alt="">
            </el-dialog>
            <el-button type="primary" class="submit">提交</el-button>
        </el-row>
        <div class="pinglunmodel">
            <el-row class="pinglun"
            v-for='(item,index) in dataList'
            :key='index'   
            @mouseleave="handleIsShow" >
                <el-row type='flex' justify="space-between">
                    <div>
                        <img src='`item.account.defaultAvatar`' alt="">
                        <span>{{item.account.nickname}}</span>
                        <span></span>
                    </div>
                    <span>{{total}}</span>
                </el-row>
                <div class="contents" 
                
                    @mouseover="handleIsShow">{{item.content}}
                </div>
                <el-row type='flex' justify="end" class="reply">
                    <nuxt-link to="#"  v-if='isShow'>
                        回复
                    </nuxt-link>
                </el-row>


            </el-row>
        </div>    







        <!-- 分页 -->
        <el-row type='flex' justify="center">
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pageIndex"
                :page-sizes="[2,4,6,8]"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="total">
            </el-pagination>
        </el-row>
    </div>
</template>

<script>
export default {
    data(){
        return{
            dataList: [],    // 分页之后的数据列表
            isShow:false,
            // input:'',
            dialogImageUrl: '',
            dialogVisible: false,
            pageIndex: 1,//默认的页面 随着页面的切换而变化  
            pageSize:5,//页面显示条数
            total:0,//总条数
            users:{},
            
        }
    },
    mounted(){
        this.$axios({
            url:'/posts/comments',
          
        }).then(res=>{
            // console.log(res.data);
            const {data, total}=res.data;
           this.users=data;
           //总条数
           this.total=total;
            // 初始化数据
            this.pageIndex = 1;
            this.dataList=data.slice(0,2);
          
        })
    },
    
    methods: {
         //上传多张图片
        // 删除后的事件处理函数
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      // 图片的预览
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      },
      //显示条数
      handleSizeChange(value){
       this.pageSize=value;
      },
          // 页数切换时候触发
      handleCurrentChange(value){
          this.pageIndex=value;
          this.setDataList();
      },
        // 修改dataList
        setDataList(arr){

            if(arr){
                this.users = arr;
                // 初始化分页变量
                this.total = arr.length;
                this.pageIndex = 1;
            }

            // 计算截图列表的数据
            this.dataList = this.users.slice(
                (this.pageIndex - 1) * this.pageSize,
                this.pageIndex * this.pageSize
            );
        },
      //鼠标的移入
      handleIsShow(){
          this.isShow=!this.isShow;
        }
  
    }
}
</script>

<style scoped lang='less'>
    .photo-wall{
        /deep/.el-upload--picture-card{
            width: 100px;
            height: 100px;
            line-height: 100px;
        }
        .submit{
            padding: 0;
            height: 24px;
            width: 56px;
        }
       
    }
    .pinglunmodel{
         border: 1px solid #ccc;
        .pinglun{
       
        margin-top: 30px;
        padding: 15px 10px 10px;
        height: 93px;
         border-bottom: 1px dashed #ccc;
        
        .reply{
           
            padding-bottom: 10px;
          

            a{
                color:skyblue;
            //    visibility: hidden;
                
            }
        }
    }
    }
</style>
