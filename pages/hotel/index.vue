<template>
  <div class="container">
    <!-- 导航栏 -->
    <div>
      <Carousel />
    </div>

    <!-- :start="_start" :limit="_limit" -->
    <!-- 筛选栏+展示栏 -->
    <div>
       
      <List :data="dataList" :hoteloption="hoteloption" class="list"/>
    </div>

    <!-- 页尾 -->
    <div class="foot">
      <div>
        <el-pagination
          @size-change="handleSizeChange"
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
      //  currentPage3: 5,
      pageIndex: 1,
      pageSize: 6,
      total: 0,
      dataList: [],
      hotelData: {},
      nextStart: 0,
      total: 0,
      hoteloption:{},
      _start: "",
      _limit: ""
    };
  },
  components: { Carousel, List },
  methods: {
    handleShow(){
        this.show=!this.show
    },
    handleSizeChange(val) {
    //   console.log(`每页 ${val} 条`);
      this.pageSize = val;
    },
    handleCurrentChange(val) {
    //   console.log(`当前页: ${val}`);
      this.pageIndex = val;
      this.setDataList();
    },
    setDataList() {
        this.dataList=this.hotelData.data.slice(
            this._start,this._limit*this.pageIndex-1
        )
        // console.log(this.dataList);
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
        // console.log(this._start);
        // console.log(this.hotelData);
        this.dataList=this.hotelData.data
          this.total=res.data.total;
        //   console.log(this.dataList);
      });
    },
    getoption(){
        this.$axios({
            url:"hotels/options",
        }).then(res=>{
            // console.log(res);
            this.hoteloption=res.data
        })
    }
  },
  watch: {
    pageIndex() {
      this.getData();
    }
  },
  mounted() {
    this.getData();
    this.getoption()
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
