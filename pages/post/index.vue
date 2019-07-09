<template>
    <div class="container">
        <div class="el-row is-justify-space-between el-row--flex">
            <!-- 侧边栏 -->
            <postAside/>
            <!-- 推荐内容 -->
            <postMain/>
        </div>
    </div>
</template>

<script>
import PostAside from "@/components/post/postAside.vue"
import PostMain from "@/components/post/postMain.vue"
export default {
  components:{
      PostAside,
      PostMain,
  }
}
</script>

<style lang="less">
*{margin:0;padding:0;}
a{text-decoration:none;}
ul,ol{list-style-type:none;}
.container{
    width: 1000px;
    margin: 0 auto;
    padding: 20px 0;
}

</style>
