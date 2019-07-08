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
        <!-- 分页 -->
        <el-row type='flex' justify="center">
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pageIndex"
                :page-sizes="[5, 10, 15, 20]"
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
            // input:'',
            dialogImageUrl: '',
            dialogVisible: false,
            pageIndex: 1,//默认的页面 随着页面的切换而变化  
            pageSize:5,//页面显示条数
            total:0,//总条数
        }
    },
    mounted(){
        this.$axios({
            url:'/posts/comments',
            method:"GET"
        }).then(res=>{
            console.log(res);
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
</style>
