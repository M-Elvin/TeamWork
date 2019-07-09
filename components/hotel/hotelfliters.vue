<template>
    <el-form ref="form" :model="form" label-width="80px">

      <div style="display: flex; ">
        <transition name="el-fade-in-linear">
          <div  class="transition-box">
            <div class="tab" @click="handleClear">
              <i class="el-icon-search"></i> 精准搜索
            </div>
            <div @click="handleBack" class="Back">
            <i class="el-icon-message-solid">撤销</i>
            </div>
            <div>
              <el-tabs :tab-position="tabPosition"  class="hotel-form">
                <el-tab-pane label="酒店等级">
                  <div class="block">
                        <i class="el-icon-star-on"></i>
                       酒店等级
                  </div>
                  <div>
                    <el-form-item class="hotel-nav-select">
                      <el-select v-model="form.hotellevel" size="mini" placeholder="请选择" @change="handleHotelleve">
                        <el-option
                          v-for="item in oplevels"
                          :key="item.id"
                          :label="item.level"
                          :value="item.id"
                          
                        > {{item.name}}级酒店</el-option>
                       
                      </el-select>
                        </el-form-item>                       
                  </div>
                </el-tab-pane>
                <el-tab-pane label="住宿类型">
                  <i class="el-icon-copy-document"></i> 住宿类型
                  <div>
                    <el-form-item >
                        <el-checkbox-group :min="0" :max="1" v-model="form.hoteltype" class="form-type" @change="handleHotelType">
                              <el-row class="form-type-list">
                                  <el-checkbox  name="type"
                                  v-for="(item,index) in optype"
                                  :key="index"
                                  :label="item.name"></el-checkbox>
                              </el-row>
                        </el-checkbox-group>
                      </el-form-item>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="酒店设施">
                  <i class="el-icon-s-cooperation"></i> 房间设施
                  <div>
                    <el-form-item >
                        <el-checkbox-group :min="0" :max="1"
                        v-model="form.hotelasset" class="form-assets" @change="handleHotelAssets">
                              <el-row class="form-assets-list">
                                  <el-checkbox  name="type"
                                  v-for="(item,index) in opassets"
                                  :key="index"
                                  :label="item.name"></el-checkbox>
                              </el-row>
                        </el-checkbox-group>
                      </el-form-item>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="酒店品牌">
                  <i class="el-icon-school"></i> 酒店品牌
                  <div >
                    <el-form-item class="hotel-nav-select">

                    <el-select v-model="form.hotelbrand" size="mini" placeholder="请选择" @change="handleHotelBrand">
                      <el-option
                        v-for="item in opbrand"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id"
                      ></el-option>
                    </el-select>
                      </el-form-item>
                    
                  </div>
                </el-tab-pane>
              </el-tabs>
            </div>
          </div>
        </transition>
      </div>
    </el-form>
</template>

<script>
export default {
props:{
    hoteloption:{
      type:Object,
      defalut:{}
    },
},
  data() {
    return {
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
    handleBack(){
        // console.log(123);
        this.$router.push({
          url:"hotels"
        })
    },
    handleClear(){
      // this.form= {
      //     hoteltype: {},
      //     hotelasset:[],
      //     hotelbrand:[],
      //     hotellevel:0,
      //   }
      console.log(123)
        this.$emit("handleShow") 
    },
    handleHotelleve(value){
      // console.log(value);
      this.form.hotellevel=value
      // this.arr.hotellevel=value
      // console.log(this.arr);
      // hotellevel
      this.$router.push({
      path:"/hotel",
      query:{
        ...this.$route.query,
        hotellevel:value
      }
    })
      // this.$emit("setDataList",this.arr)
    },
    handleHotelType(value){
      this.form.hoteltype=value
      // console.log(value);
      // const arr=this.form.hoteltype
      // console.log(this.form.hoteltype[0]);
      if(value.length==0)
      {
        return;
      }
      // console.log(typeof(value));
     
        this.arr.hoteltype=this.optype.filter(v=>{
        if(v.name==value[0])
        {
          return v;
        }
      })
      // console.log(this.arr.hoteltype[0].id);
          this.$router.push({
          path:"/hotel",
          query:{
            ...this.$route.query,
            hoteltype:this.arr.hoteltype[0].id
          }
        })
      // console.log(this.arr.hoteltype[0].name,this.arr.hoteltype[0].id);
      
      // if(this.arr.hoteltype[0])
      // {
      //   this.arr.hoteltype=this.arr.hoteltype[0].id
      //   // console.log(this.arr.hoteltype);
      //   this.$emit("setDataList",this.arr)
      //   // console.log(this.arr.hoteltype[0].id);
      // }
    },
    handleHotelAssets(value){
      if(value.length==0)
      {
        return;
      }
      console.log(value);
       this.form.hotelasset=value
      //  console.log(this.form.hotelasset);
       this.arr.hotelasset=this.opassets.filter(v=>{
         if(v.name==value)
         {
           return v
         }
       })

          this.$router.push({
          path:"/hotel",
          query:{
            ...this.$route.query,
            hotelasset:this.arr.hotelasset[0].id
          }
        })
       console.log(this.arr.hotelasset[0].id);
      //  if(this.arr.hotelasset[0])
      //  {
      //    this.arr.hotelasset=this.arr.hotelasset[0].id
      //    this.$emit("setDataList",this.arr)
      //  }
      // console.log(value);
    },
    handleHotelBrand(value){
      console.log(value);
      this.arr.hotelbrand=value
      // this.$emit("setDataList",this.arr)
  
        this.$router.push({
          path:"/hotel",
          query:{
            ...this.$route.query,
          hotelbrand:value
          }
        })
    },

    handleGetoption(){
      console.log(this.hoteloption.data);
        this.optype=this.hoteloption.data.types
        this.opbrand=this.hoteloption.data.brands
        this.opassets=this.hoteloption.data.assets
        this.oplevels=this.hoteloption.data.levels
        // console.log(this.opbrand);
        // console.log(this.opassets);
        // console.log(this.optype);
        // console.log(this.hoteloption);
        // console.log(this.opbrand);
    },


  },
  watch: {
    data() {
      this.handleGetdata();
    },
    // form(){
    //   this.show=!this.show;
    // }
  },
  mounted() {
    setTimeout(() => {
    this.handleGetoption()
    this.handleGetdata();
    }, 3300);
  }

}
</script>

<style lang="less" scoped>
//精准搜索栏
.transition-box {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translateX(-50%) translateY(-50%);
  margin-bottom: 10px;
  width: 500px;
  border-radius: 4px;
  background-color: rgb(137, 152, 167);
  text-align: center;
  color: #fff;
  padding: 26px 32px;
  box-sizing: border-box;
  margin-right: 20px;
  z-index: 8;
  .tab {
    font-size: 50px;
    padding-bottom: 20px;
    margin-bottom: 20px;
    border-bottom: 1px solid #ccc;
    cursor: pointer;
  }
  .Back{
    position: absolute;
    right: 0;
    top:0;
    cursor: pointer;
  }
}
.hotel-form {
  font-size: 26px;

  /deep/ .el-rate {
    height: 27px;
    margin-top: 20px;
    font-size: 28px;
  }
  /deep/ .el-tabs__item {
    color: rgba(102, 57, 11, 0.685);
    &:hover {
      color: #fcc;
    }
  }
  /deep/ .el-tabs__active-bar {
    background-color: rgba(136, 40, 11, 0.692);
  }
  // 酒店类型
  /deep/ .el-checkbox__input.is-checked + .el-checkbox__label {
    color: rgba(102, 57, 11, 0.685);
  }
  .el-checkbox.is-bordered.is-checked {
    border-color: #ddd;
  }
  /deep/.el-checkbox__input.is-checked .el-checkbox__inner,
  .el-checkbox__input.is-indeterminate .el-checkbox__inner {
    border-color: #ddd;
    background-color: rgba(240, 128, 128, 0.479);
  }
  .hotel-nav-select {
    margin: 20px 0;
    margin-right: 70px;
    span {
      display: block;
      margin-bottom: 6px;
    }
    /deep/ .el-input__inner {
      width: 100px;
    }
  }
  .form-type,.form-assets{
    .form-type-list{
      width: 300px;
      margin-top: 10px;
      transform: translateX(-50px);
          .el-checkbox{
              &:nth-child(2n-1){
                float: left;
          }
            &:nth-child(2n){
                float: right;
          }
          &:last-child{
            margin-right: 14px; 
          }
        }
    }
  }
  .form-assets-list{
      width: 260px;
      margin-top: 10px;
          .el-checkbox{
              &:nth-child(2n-1){
            width: 15%;
                float: left;
          }
            &:nth-child(2n-2){
            width: 50%;
                float: right;
                margin-left: 0;
                text-align-last: left;
          }
          &:last-child{
            margin-right: 30px;
          }
        }
  }
}
</style>
