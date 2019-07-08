<template>
  <div class="container">
    <!-- 幻灯片 -->
    <div>
      <el-carousel :interval="5000" arrow="never">
        <el-carousel-item v-for="(item, index) in banners" :key="index">
          <div
            class="banner-image"
            :style="`
                    background:url(${item.url}) center center no-repeat;
                    background-size:cover;
                    `"
          ></div>
        </el-carousel-item>
      </el-carousel>
    </div>

    <!-- 头部查询 -->
    <!-- <span @click="handleShow"><i class="el-icon-bell"></i></span> -->
    <div class="search-form">
      <!-- 头部tab切换 -->

      <el-form class="search-form-content" ref="form">
        <div class="search-tab">
          <el-row type="flex" justify="space-around" class="search-tab">
            <!-- tab切换栏 -->
            <span
              v-for="(item, index) in options"
              :key="index"
              :class="{active: currentOption == index}"
              @click="handleOption(index)"
            >
              <i>{{ item.name }}</i>
            </span>
          </el-row>
        </div>
        <div>
          <el-row type="flex" justify="center" class="search-form-head">
            <i class="el-icon-school"></i>
            <div>
              <span>万家酒店豪礼等着你</span>
              <span>(省内/省外)</span>
            </div>
          </el-row>
        </div>
        <el-form-item class="search-form-inp">
          <div>
            <el-autocomplete
              size="mini"
              :fetch-suggestions="queryCitySearch"
              @select="handleCitySelect"
              v-model="form.citiesname"
              placeholder="示例: 南京市"
            >
              <i slot="prefix" class="el-input__icon el-icon-search"></i>
            </el-autocomplete>
            <div class="main-tar">商圈、地表、关键字</div>
          </div>
        </el-form-item>

        <!-- 订单列表 -->
        <div class="user_msg">
          <el-row type="flex" class="user_msglist">
            <el-form-item label class="search-form-room">
              <div class="room-left" v-if="RoomisShow">
                <!-- 类型 -->
                <div>房间类型</div>
                <el-select v-model="value_room" placeholder="请选择">
                  <el-option-group >
                    <el-option
                      v-for="item in optype"
                      :key="item.id"
                      :label="item.name"
                      :value="item.name"
                    ></el-option>
                  </el-option-group>
                </el-select>
                <!-- 人数 -->
                <div>入住人数</div>
                <el-select v-model="value_num" placeholder="请选择">
                  <el-option
                    v-for="item in roomnum"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  ></el-option>
                </el-select>
              </div>
              <div class="time-left" v-if="TimeisShow">
                <div class="block">
                  <span class="demonstration">入住时间</span>
                  <el-date-picker
                    size="small"
                    v-model="intime"
                    type="datetime"
                    align="center"
                    placeholder="入住日期"
                  ></el-date-picker>
                </div>
              </div>
            </el-form-item>

            <el-form-item label class="search-form-roomnum">
              <div class="room-right" v-if="RoomisShow">
                <div>酒店星级</div>
                <el-rate v-model="star"></el-rate>
              </div>
              <div class="time-right" v-if="TimeisShow">
                <div class="block">
                  <span class="demonstration">退房时间</span>
                  <el-date-picker
                    v-model="outtime"
                    size="small"
                    align="center"
                    type="datetime"
                    placeholder="退房日期"
                  ></el-date-picker>
                </div>
              </div>
            </el-form-item>
          </el-row>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
   props:{
       data:{
           type:Array,
           default:[]
       },
       hoteloption:{
           type:Object,
           default:{}
       }
   },
  data() {
    return {
      // 轮播图数据
      form: {
        citiesname: "",
        citiesid:{},
      },
      star: null,
      RoomisShow: true,
      TimeisShow: false,
      intime: "",
      outtime: "",
      optype:[],
      currentOption: 0,
      banners: [
        {
          url:
            "https://www.shangri-la.com/uploadedImages/Shangri-la_Hotels/SC_Owned_Channel_Asset_Family_Asset_Shangrila_Site_HomepageBanner_1920_800.jpg"
        },
        {
          url:
            "https://www.shangri-la.com/uploadedImages/Shangri-la_Hotels/New_Website/Banner-1920-x-800.jpg"
        },

        {
          url:
            "https://www.shangri-la.com/uploadedImages/Shangri-la_Hotels/1920X800.jpg"
        },
        {
          url:
            "https://www.shangri-la.com/uploadedImages/Shangri-la_Hotels/MB_SC5.jpg"
        },
        {
          url: "http://image.wyn88.com/adminpro/201811161811037040.jpg"
        }
      ],
      options: [
        {
          name: "入住类型",
        },
        {
          name: "入住时间",
        }
      ],
      value_room: "",
      value_num: "",
      roomnum: [
        {
          value: 1,
          label: "一个人"
        },
        {
          value: 2,
          label: "两个人"
        },
        {
          value: 3,
          label: "三个人"
        },
        {
          value: 4,
          label: "四个人"
        }
      ],
     
    };
  },
  methods: {
    handleShow() {
      // console.log(123);
      // this.isShow=!this.isShow
    },
    handleOption(index) {
      this.currentOption = index;
      if (index == 0) {
        this.TimeisShow = false;
        this.RoomisShow = true;
      } else {
        this.TimeisShow = true;
        this.RoomisShow = false;
      }
    },
    // 目标城市输入框获得焦点时触发
    // value 是输入框的值，cb是回调函数，接收要展示的列表
    queryCitySearch(value,cb) {
      this.$axios({
        url: "cities",
        params: {
          name: `${value}`
        }
      }).then(res => {
        const { data } = res.data;
        // console.log(data);
        const newData = data.map(v => {
          return {
            ...v,
            value: v.name
          };
        });
        // this.form.citiesname=newData[0].value
        cb(newData);
      });
    },
    handleCitySelect(item) {
    // console.log(item);
    this.$router.push({
      path:"/hotel",
      query:{
        ...this.$route.query,
        city:item.id
      }
    })

    },
    handleGetoption(){
    this.optype=this.hoteloption.data.types

    // console.log(this.opbrand);
    // console.log(this.opassets);
    // console.log(this.optype);
    // console.log(this.hoteloption);
    // console.log(this.opbrand);
    },

  },
    mounted() {
    setTimeout(() => {
    this.handleGetoption()
    }, 600);
  }
};
</script>

<style scoped lang="less">
.container {
  min-width: 1000px;
  margin: 0 auto;
  position: relative;
  /deep/ .el-carousel__container {
    height: 490px;
  }
  .banner-image {
    width: 100%;
    height: 100%;
  }
  .search-form {
    position: absolute;
    border-radius: 10px;
    background-color: rgba(29, 24, 24, 0.438);
    border: 1px rgb(66, 66, 66) solid;
    border-top: none;
    width: 360px;
    height: 460px;
    box-sizing: border-box;
    top: 0px;
    left: 26px;
    z-index: 2;
    .search-form-content {
      margin: 0 auto;
      padding: 3px 18px;
      // overflow: hidden;
      box-sizing: border-box;
      .search-form-head {
        color: rgba(206, 188, 238, 0.781);
        padding: 10px;
        font-size: 20px;
        font-weight: 800;
        span {
          &:nth-child(2) {
            font-size: 12px;
            font-weight: 600;
          }
        }
        > i {
          font-size: 30px;
        }
      }
      .search-form-inp {
        .main-tar {
          line-height: 20px;
          color: #ccc;
          margin-bottom: 10px;
        }
        /deep/ .el-input--mini .el-input__inner {
          width: 312px;
        }
      }
      .user_msg {
        width: 312px;
        height: 200px;
        top: 29px;
        border-radius: 10px;
        background-color: #fff;
        position: relative;
        .user_msglist {
          height: 100%;
        }
      }
      .search-form-room {
        width: 50%;
        text-align: center;
        margin: 6px 0;
        border-right: 1px dashed #ccc;
        .room-left {
          width: 100px;
          margin: 0 auto;
        }
      }
      .search-form-roomnum {
        width: 50%;
        text-align: center;
        margin: 6px 0;
        .room-right {
          text-align: center;
          font-size: 26px;
          line-height: 90px;
          width: 150px;
          margin: 0 auto;
        }
      }
      .time-left {
        width: 100px;
        margin: 0 auto;
        /deep/ .el-input--prefix .el-input__inner {
          width: 113px;
        }
      }
      .time-right {
        width: 100px;
        margin: 0 auto;
        /deep/ .el-input--prefix .el-input__inner {
          width: 113px;
        }
      }
      // .search-form-main,
      // .search-form-time,
      // .search-form-room{
      // border-bottom: 1px solid #ccc;
      // box-shadow: 0 1px #f5f4f0;
      // }
    }
    .hotle-intime,
    .hotle-outtime {
      width: 130px;
      height: 230px;
    }
  }

  // tab栏
  @keyframes run {
    0% {
      transform: translateX(0px);
    }
    50% {
      transform: translateY(-200px);
    }
    100% {
      transform: translateX(300px);
    }
  }
  .search-tab {
    position: absolute;
    // left: -20px;
    /* animation: name duration timing-function delay iteration-count direction fill-mode; */
    // animation: run 2s alternate-reverse infinite linear;
  }
  .search-tab {
    width: 80%;
    top: 30%;
    left: 10%;
    z-index: 3;
    .active {
      i {
        color: #333;
      }
      &::after {
        background: #eee;
      }
    }

    span {
      width: 82px;
      height: 36px;
      display: block;
      position: relative;
      margin-right: 8px;
      cursor: pointer;

      i {
        position: absolute;
        z-index: 2;
        display: block;
        width: 100%;
        height: 100%;
        line-height: 30px;
        text-align: center;
        color: #fff;
      }

      &:after {
        position: absolute;
        left: 0;
        top: 0;
        display: block;
        content: "";
        width: 100%;
        height: 100%;
        border: 1px rgba(255, 255, 255, 0.2) solid;
        border-bottom: none;
        transform: scale(1.1, 1.3) perspective(0.7em) rotateX(2.2deg);
        transform-origin: bottom left;

        background: rgba(0, 0, 0, 0.5);
        border-radius: 1px 2px 0 0;
        box-sizing: border-box;
      }
    }
  }
}
</style>