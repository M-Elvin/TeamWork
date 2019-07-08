<template>
  <div class="container">
    <!-- 导航栏 -->
    <div>
      <Carousel :data="dataList" 
       :hoteloption="hoteloption" 
       @setDataList="setDataList"
       />
    </div>

    <!-- 筛选栏+展示栏 -->
    <div>
      <List :data="dataList" 
      :hoteloption="hoteloption"
       class="list"
       @setDataList="setDataList"
       />
    </div>

    <!-- 页尾 -->
    <div class="foot">
      <div>
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page.sync="pageIndex"
          :page-size="pageSize"
          layout="prev, pager, next, jumper"
          :total="total"
        ></el-pagination>
      </div>
    </div>
    <div style="height:60px; width:100%"></div>
  </div>
</template>

<script>
import Carousel from "@/components/hotel/carousel.vue";
import List from "@/components/hotel/list.vue";
import SearchForm from "@/components/hotel/searchForm.vue"
export default {
  data() {
    return {
      pageIndex: 1,
      pageSize: 10,
      total: 0,
      dataList: [],
      hotelData: {},
      nextStart: 0,
      total: 0,
      hoteloption:{},
      _start: "",
      _limit: "",
      formpageIndex:0,
      star:{hotellevel:0},
      cityid:{city:0}
    };
  },
  components: { Carousel, List },
  methods: {
    handleShow(){
        this.show=!this.show
    },
    handleCurrentChange(val) {
    //   console.log(`当前页: ${val}`);
      this.pageIndex = val;
      if(this.$route.query)
      {
        this.setDataList()
      }else{    
          this.getData()     
      }
    },
    // handleStart(){
    //   if(arr)
    //   {
    //     this.setDataList(star)
    //   }
    // },

    setDataList() {
      console.log(this.$route.query)
        this.$axios({
        url:"/hotels",
        params:{
          _limit: this.pageSize,
          _start: (this.pageIndex - 1) * this.pageSize,
          ...this.$route.query
        }
      }).then(res=>{
        this.hotelData = res.data;
        this.dataList=this.hotelData.data
        this.total=res.data.total;
      })
    },
    getData() {
      this.$axios({
        url: "/hotels",
        params: {
          _limit: this.pageSize,
          _start: (this.pageIndex - 1) * this.pageSize
        }
      }).then(res => {
        // console.log(res.data);
        this.hotelData = res.data;
        this._start = this.pageSize;
        // console.log(this.hotelData);
        this.dataList=this.hotelData.data
          this.total=res.data.total;
          // console.log(this.dataList);
      });
    },
    getoption(){
        this.$axios({
            url:"hotels/options",
        }).then(res=>{
            // console.log(res);
            this.hoteloption=res.data
            // console.log(this.hoteloption);
        })
    },
    getCity(){
        this.$axios({
          url:"cities",
          params:{
            name:""
          }
          }).then(res=>{
            // console.log(res);
        })
    } 
  },
  watch: {
    pageIndex() {
          if(!(this.$route.query))
          {
            this.getData();
          }
    },
    $route(){
      this.setDataList()
    },
    formpageIndex(){
      this.pageIndex=1;
    }
      
  
  },
  mounted() {
    this.getData();
    this.getoption()
    this.getCity()
  }
};
</script>

<style lang="less" scoped>
.container {
  position: relative;
  overflow: hidden;
  .foot {
    margin-top: 20px;
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
  }

  .list{
      z-index: 6;
  }
}
</style>
