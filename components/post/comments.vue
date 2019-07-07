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
                :on-success="handleCartSuccess"
                :on-preview="handlePictureCardPreview"
                :on-remove="handleRemove">
                
                <i class="el-icon-plus"></i>
            </el-upload>
            <el-dialog :visible.sync="dialogVisible">
                <img width="100%" :src="dialogImageUrl" alt="">
            </el-dialog>
            <el-button type="primary" class="submit">提交</el-button>
        </el-row>
        <!-- 评论内容展示框 -->
        <el-row class="pinglun">
            <div class="pingluns">
                <el-row 
                class="pinglun-title"
                type="flex" 
                justify="space-between"> 
                    <div>
                        <span>头像</span>
                        <span>用户名</span>
                        <span>时间</span>
                    </div>
                    <span >1</span>
                </el-row>
                <p>内容</p>
                <div class="reply" >
                    <a href="javascript:;">回复</a>
                </div>
           </div>
        </el-row>





        <!-- 分页 -->
        <el-row type='flex' justify="center">
             <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="4"
                :page-sizes="[100, 200, 300, 400]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="400">
                </el-pagination>
        </el-row>

    </div>
</template>

<script>
export default {
    data(){
        return{
            dialogImageUrl: '',
            dialogVisible: false,
            pageIndex: 1,//默认的页面 随着页面的切换而变化  
            pageSize:5,//页面显示条数
            total:0,//总条数
            fileList: [], // 多张图片
        }
    },

    mounted(){
        console.log(this.$route.params);
        //获取文章评论
        this.$axios({
            url:`/posts/comments`,
        }).then(res=>{
            console.log(res.data);
        })
    },
    
    methods: {
         //上传多张图片
        handleCartSuccess(res, file, fileList){
            console.log(fileList);
             // 把fileList的数组中的每一项中response属性提取出来
              const files = fileList.map(v => {
                return v.response;
            });
             this.fileList = files;
        },
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
            console.log(value);
        },
        handleCurrentChange(value){
            console.log(value);
        },
  
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
    .pinglun{
        margin-top: 30px;
        border: 1px solid #ccc;
        padding: 10px;
        .pingluns{
            border-bottom:1px dashed #ccc; 
            .pinglun-title{
                color:#717278;
                
            }
           
            .reply{
                display: flex;
                justify-content: end;
                color:skyblue;
                // display: none;
               
            }
        }
       
    }
</style>
