<template>
        <div style="padding:50px;">
            高德地图
            <div id="container">
            </div> 
            <div id="panel"></div>
            <div>
                <select v-model="city">
                    <option value="广州">广州</option>
                </select>
                <input type="text" v-model="start" placeholder="起点位置">
                <input type="text" v-model="end" placeholder="你要去哪">
                <button @click="handleSearch">开车</button>
                <button @click="handleSearch">公交</button>
            </div>
        </div>
</template>

<script>
export default {
    data(){
        return{
            city:'',
            start:'',
            end:'',
            map:null

        }
    },
    methods:{
        //开车
        handleSearch(){
              this.map = new AMap.Map('container', {
                zoom:11,//放大级别
            });
            //查询乘车路线
             var driving = new AMap.Driving({
                // 驾车路线规划策略，AMap.DrivingPolicy.LEAST_TIME是最快捷模式
                policy: AMap.DrivingPolicy.LEAST_TIME,
                map:this.map,
                panel:'panel'
            })
            
            var points = [
                { keyword: this.start,city:this.city },
                { keyword: this.end,city:this.city }
            ]
            
            driving.search(points, function (status, result) {
                // 未出错时，result即是对应的路线规划方案
                console.log(result);
            })
                    },
                },
    mounted(){
        //在页面加载完毕后执行
        window.onLoad  = function(){
            //引入插件
            var map = new AMap.Map('container',{
              
                zoom:12,//放大级别
                center:[116.397428, 39.90923],//中心点坐标
                viewMode:'3D'//使用3D视图
            });
            this.map=map;
              // 工具条插件
            var toolbar = new AMap.ToolBar();
            map.addControl(toolbar);
            // 创建一个 Marker 实例：
            
            // var marker1 = new AMap.Marker({
            //     //自定义图片内容
            //     content:`<div class="marker-route marker-marker-bus-from">1</div>`,
            //     position: new AMap.LngLat(116.39, 39.9),   // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
            //     title: '北京'
            // });
            // var marker2 = new AMap.Marker({
            //      content:`<div class="marker-route marker-marker-bus-from">2</div>`,
            //     position: new AMap.LngLat(116.393, 39.93),   // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
            //     title: '东京'
            // });

            // // 将创建的点标记添加到已有的地图实例：
            // map.add([marker1,marker2]);
            



        
        }
        //引入高德地图JS文件
        var url = `https://webapi.amap.com/maps?v=1.4.15&key=${'5d2f751be105aa5e6137ecf793874c92'}&callback=onLoad&plugin=AMap.ToolBar,AMap.Driving`;
        var jsapi = document.createElement('script');
        jsapi.charset = 'utf-8';
        jsapi.src = url;
        document.head.appendChild(jsapi);
        
    }
}   
</script>
 
<style > 
    #container{
       
      
        width: 500px;
        height: 500px;
     
    }
      #panel {
            position: fixed;
            background-color: white;
            max-height: 90%;
            overflow-y: auto;
            top: 10px;
            right: 10px;
            width: 280px;
        }
        #panel .amap-call {
            background-color: #009cf9;
            border-top-left-radius: 4px;
   	        border-top-right-radius: 4px;
        }
        #panel .amap-lib-driving {
	        border-bottom-left-radius: 4px;
   	        border-bottom-right-radius: 4px;
            overflow: hidden;
        }
</style>
