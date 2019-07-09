<template>
    <div id="app">
        <el-row :gutter="20">
            <el-col :span="11" :offset="5">
                <div class="grid-content bg-purple">
                    <div class="title1">发表新攻略</div>
                    <div class="title2">分享你的个人游记，让更多人看到哦！</div>
                    <el-input v-model="form.title" placeholder="请输入标题"></el-input>
                    <div v-if="!$isServer" class="fuwenb"><VueEditor class="VueEditor" :config="config" ref="vueEditor"/></div>
                    <div class="demo-input-suffix">选择城市
                      <el-autocomplete
                        :fetch-suggestions="queryDepartSearch"
                        placeholder="请搜索出发城市"
                        @select="handleDepartSelect"
                        class="el-autocomplete cityValue"
                        v-model="form.departCity"
                        ></el-autocomplete></div>
                    <div class="bottom">
                        <el-button type="primary">发布</el-button>
                        <span class="or">或者</span>
                        <el-link type="warning" @click="handleAddDraft">保存到草稿箱</el-link>
                    </div>
                </div>
            </el-col>
            <el-col :span="4" :offset="1">
                <div class="grid-content bg-purple">
                    <div>
                        <div>草稿箱(0)</div>
                        
                    </div>
                </div>
            </el-col>
        </el-row>

    </div>
</template>

<script>
// require styles
import 'quill/dist/quill.snow.css'
 let VueEditor;

if (process.browser) {
    VueEditor = require('vue-word-editor').default
}

export default {
    name: 'app',
data(){
  return {
      
      form: {
              departCity: "", // 出发城市
              departCode: "", // 出发城市代码
              title:""
              
            },
      

    
    
    config: {
      modules: { 
        // 工具栏
        toolbar: [
          ['bold', 'italic', 'underline', 'strike'],        // toggled buttonso
          ['blockquote', 'code-block'],
          ['image', 'video'],

          [{ 'header': 1 }, { 'header': 2 }],               // custom button values
          [{ 'list': 'ordered'}, { 'list': 'bullet' }],
          [{ 'script': 'sub'}, { 'script': 'super' }],      // superscript/subscript
          [{ 'indent': '-1'}, { 'indent': '+1' }],          // outdent/indent
          [{ 'direction': 'rtl' }],                         // text direction
        ]
      },
      // 主题
      theme: 'snow',
      uploadImage: {
        url: "http://localhost:1337/upload",
        name: "files",
        uploadBefore(file){
          return true
        },
        uploadProgress(res){

        },
        uploadSuccess(res, insert){
          insert("http://localhost:1337" + res.data[0].url)
        },
        uploadError(){},
        showProgress: false
      },

      uploadVideo: {
        //url: "http://157.122.54.189:9095/upload",
        url: "http://localhost:1337/upload",
        name: "files",
        uploadBefore(file){
          return true
        },
        uploadProgress(res){

        },
        uploadSuccess(res, insert){
          insert("http://localhost:1337" + res.data[0].url)
        },
        uploadError(){},
      }
    }
  }
},
methods: {
       
        
        
        queryDepartSearch(value , cb){
            if(!value) {
                return;
            }

            this.$axios({
                url: "/airs/city?name=" + value,
                method: "GET"
            }).then(res => {
                const {data} = res.data;
                const newData = data.map(v => {
                    return {
                        ...v,
                        value: v.name.replace("市", "")
                    }
                });
                
                // 如果用户不在下拉选项中选择，默认选择第一个
                this.form.departCity = newData[0].value;
                this.form.departCode = newData[0].sort;

                // cb函数接收的参数是数组，数据里面每一项必须是对象，然后带有value的属性
                cb(newData);
            });
        },
        // 出发城市下拉选择时触发
        handleDepartSelect(item) {
            this.form.departCity = item.value;
            this.form.departCode = item.sort;
        },

        // 目标城市下拉选择时触发
        handleDestSelect(item) {
            this.form.destCity = item.value;
            this.form.destCode = item.sort;
        },
        handleAddDraft(){
            if(!this.form.title||!this.$refs.vueEditor.editor.root.innerHTML||!this.form.departCity){
              this.$message.warning("请输入完整信息")
              return
            }
            const obj ={...this.form};

            this.draft =JSON.parse(localStorage.getItem("airs")||"[]");
            this.draft.unshift(obj);
            localStorage.setItem("airs",JSON.stringify(this.draft));
            this.form.title="";
            this.form.departCity="";
            this.$refs.vueEditor.editor.root.innerHTML="";
            console.log(this.draft);
        }  
        },
components: {
  VueEditor
},
  }

</script>

<style lang="less" scoped>
    .el-input{
        margin: 10px 0 ;
    }
    .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
  }
  .cityValue{
      width: 200px;
      margin-left: 10px;
  }
  .title1{
      font-size:25px;
      margin: 40px 0 15px;
  }
  .title2{
      font-size:14px;
      color: #666;
      
  }
  .fuwenb{
    height: 450px;
  }
  .VueEditor{
      height: 400px;
      margin: 15px 0;
  }
  .demo-input-suffix{
      margin: 20px 0;
      font-size: 14px;
  }
  .bottom{
      margin-bottom: 20px;
  }
  .or{
      font-size: 14px;
      margin-left: 10px;
  }
  .grid-content{
      margin-top: 40px;
  }
</style>
