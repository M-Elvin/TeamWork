<template>
  <div class="container">

    <HotelFliter class="show" v-if="isShow"
    :hoteloption="hoteloption"/>
    <div class="banner"><i class="el-icon-s-promotion search
        " @click="handleShow"></i>精选推广</div>
    <div class="hotel-item" v-id="load">
      <div v-for="(item,index) in dataList" :key="index">
        <div class="block">
          <div class="check-hotel">
            <a href="https://hotels.ctrip.com/hotel/694679.html">查看详情</a>
            <div class="topline"></div>
            <div class="bottomline"></div>
            <div class="leftline"></div>
            <div class="rightline"></div>
          </div>
          <el-image class="hotel-img" :src="url"></el-image>
          <div class="list-hotel">
            <span>{{item.name}}</span>
            <span>
              <i class="el-icon-location"></i>
              位于:{{item.address}}
            </span>
            <span>{{item.hoteltype.name}}酒店</span>
            <span>仅￥{{item.price}}起</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HotelFliter from '@/components/hotel/hotelfliters.vue'
export default {
  props: {
    data: {
      type: Array,
      default: []
    },
    hoteloption:{
      type:Object,
      defalut:{}
    }
  },
  components:{HotelFliter},
  data() {
    return {
      load:true,
      isShow:false,
      arr:[],
      brand:"",
      level:1,
      hotel_type: [],
      hotel_op: [],
      tabPosition: "left",
      url:
        "https://www.shangri-la.com/uploadedImages/Shangri-la_Hotels/1920X800.jpg",
      // fits: ['fill', 'contain', 'cover', 'none', 'scale-down'],
      // hotel_item:[
      //   {url: 'https://www.shangri-la.com/uploadedImages/Shangri-la_Hotels/1920X800.jpg',
      //   type:"经济型",
      //   price:"61",
      //   name:"好来阁商务宾馆",
      //   redress:"高淳县淳溪镇镇兴路118号(高淳县委党校对面)",
      //   friendlink:'https://hotels.ctrip.com/hotel/694679.html'
      //   }]
      dataList: [],
      optype:[],
      opbrand:[],
      opassets:[],
      oplevels:[],
      //表单
        form: {
          hoteltype: [],
          hotelasset:[],
          hotelbrand:[],
          hotellevel:0,
        },
    };
  },
  methods: {
    handleShow(){
      this.isShow=!this.isShow
      // console.log(123456);
    },
    // handleClear(){
    //   this.form= {
    //       hoteltype: {},
    //       hotelasset:[],
    //       hotelbrand:[],
    //       hotellevel:0,
    //     }
    // },
    handleGetdata() {
        this.dataList = this.data;
        console.log(this.dataList);
        // if(this.dataList)
        // {
        //   this.load=true;
        // }
    },

  },
  watch: {
    data() {
      this.handleGetdata();
      this.isShow=false;
    },

  },
  mounted() {
    setTimeout(() => {
    // this.handleGetoption()
    this.handleGetdata();
    }, 1200);
  }
};
</script>

<style lang="less" scoped>
.container {
  min-width: 1200px;
  position: relative;
  .show{
    z-index: 20;
  }
  .search{
    // position: absolute;
    // top: 10%;
    // left:0;
    font-size: 30px;
    color: rgb(240, 149, 13);
    cursor: pointer;
    z-index: 8;
  }
  .hotel-item {
    // display: flex;
    width: 100%;
    // flex-wrap: wrap;
    // justify-content: space-between;
    // align-items: center;
    margin: 0 auto;
    .block {
      box-sizing: border-box;
      // margin: 6px 0;
      &:nth-child(2n-1) {
        float: left;
        width: 50%;
      }
      &:nth-child(2n) {
        .hotel-img {
          float: right;
          width: 50%;
        }
      }
      // height: 166px;
      // margin-bottom: 12px;
      position: relative;
      .check-hotel {
        opacity: 0;
        cursor: pointer;
        height: 180px;
        width: 460px;
        background: rgba(219, 211, 211, 0.637);
        position: absolute;
        transform: translateX(-50%) translateY(-50%);
        text-align: center;
        line-height: 180px;
        font-size: 48px;
        color: rgba(75, 137, 218, 0.808);
        // margin-left: -190px;
        left: 50%;
        top: 50%;
        z-index: 6;
        .topline {
          top: 0;
          left: 0;
          height: 3px;
          width: 0;
          transition: all 1.2s;
        }
        .bottomline {
          bottom: 0;
          right: 0;
          height: 3px;
          width: 0;
          transition: all 1.2s;
        }
        .leftline {
          bottom: 0;
          left: 0;
          width: 3px;
          height: 0;
          transition: all 1.2s;
        }
        .rightline {
          top: 0;
          right: 0;
          width: 3px;
          height: 0;
          transition: all 1.2s;
        }
        &:hover {
          transition: all 1s;
          opacity: 1;
          .topline,
          .bottomline,
          .leftline,
          .rightline {
            background-color: #051f38;
            position: absolute;
          }
          .topline {
            top: 0;
            left: 0;
            height: 3px;
            width: 100%;
          }
          .bottomline {
            bottom: 0;
            right: 0;
            height: 3px;
            width: 100%;
          }
          .leftline {
            bottom: 0;
            left: 0;
            width: 3px;
            height: 100%;
          }
          .rightline {
            top: 0;
            right: 0;
            width: 3px;
            height: 100%;
          }
        }
      }
      .list-hotel {
        position: absolute;
        bottom: 3px;
        background-color: rgba(4, 12, 36, 0.582);
        height: 40%;
        width: 100%;
        color: #ccc;
        font-size: 30px;
        z-index: 5;
        span {
          position: absolute;
          right: 0;
          top: 30%;
          &:nth-child(2) {
            font-size: 13px;
            top: 80%;
            color: rgba(199, 199, 199, 0.664);
          }
          &:nth-child(3) {
            font-size: 18px;
            left: 0;
            top: 0;
          }
          &:nth-child(4) {
            font-size: 23x;
            top: 50%;
            left: 0;
            color: rgba(240, 142, 30, 0.795);
          }
        }
      }
    }
  }
  .banner {
    width: 100%;
    height: 60px;
    background-color: #051f38;
    text-align: center;
    color: #fff;
    line-height: 60px;
    font-size: 30px;
  }
}
//精准搜索栏
// .transition-box {
//   position: absolute;
//   left: 50%;
//   top: 50%;
//   transform: translateX(-50%) translateY(-50%);
//   margin-bottom: 10px;
//   width: 500px;
//   border-radius: 4px;
//   background-color: rgb(137, 152, 167);
//   text-align: center;
//   color: #fff;
//   padding: 26px 32px;
//   box-sizing: border-box;
//   margin-right: 20px;
//   z-index: 8;
//   .tab {
//     font-size: 50px;
//     padding-bottom: 20px;
//     margin-bottom: 20px;
//     border-bottom: 1px solid #ccc;
//     cursor: pointer;
//   }
// }
// .hotel-form {
//   font-size: 26px;

//   /deep/ .el-rate {
//     height: 27px;
//     margin-top: 20px;
//     font-size: 28px;
//   }
//   /deep/ .el-tabs__item {
//     color: rgba(102, 57, 11, 0.685);
//     &:hover {
//       color: #fcc;
//     }
//   }
//   /deep/ .el-tabs__active-bar {
//     background-color: rgba(136, 40, 11, 0.692);
//   }
//   // 酒店类型
//   /deep/ .el-checkbox__input.is-checked + .el-checkbox__label {
//     color: rgba(102, 57, 11, 0.685);
//   }
//   .el-checkbox.is-bordered.is-checked {
//     border-color: #ddd;
//   }
//   /deep/.el-checkbox__input.is-checked .el-checkbox__inner,
//   .el-checkbox__input.is-indeterminate .el-checkbox__inner {
//     border-color: #ddd;
//     background-color: rgba(240, 128, 128, 0.479);
//   }
//   .hotel-nav-select {
//     margin: 20px 0;
//     margin-right: 70px;
//     span {
//       display: block;
//       margin-bottom: 6px;
//     }
//     /deep/ .el-input__inner {
//       width: 100px;
//     }
//   }
//   .form-type,.form-assets{
//     .form-type-list{
//       width: 300px;
//       margin-top: 10px;
//       transform: translateX(-50px);
//           .el-checkbox{
//               &:nth-child(2n-1){
//                 float: left;
//           }
//             &:nth-child(2n){
//                 float: right;
//           }
//           &:last-child{
//             margin-right: 14px; 
//           }
//         }
//     }
//   }
//   .form-assets-list{
//       width: 260px;
//       margin-top: 10px;
//           .el-checkbox{
//               &:nth-child(2n-1){
//             width: 15%;
//                 float: left;
//           }
//             &:nth-child(2n-2){
//             width: 50%;
//                 float: right;
//                 margin-left: 0;
//                 text-align-last: left;
//           }
//           &:last-child{
//             margin-right: 30px;
//           }
//         }
//   }
// }
</style>
