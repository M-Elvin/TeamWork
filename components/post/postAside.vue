<template>
    <div class="menus-wrapper">
        <!-- 侧栏 -->
        <div class="menus-body" @mouseleave="sublist = false;current=5" >
            <ul class="menus">

                <li class="menu-item" 
                @mouseenter="handleEnt(index,item)" 
                v-for="(item,index) in cityList" 
                :key="index" :class="{ active:index==current}">{{item.type}}
                    
                </li>
            </ul>
            <!-- 子级 -->
            <div class="sub-menus" v-if="sublist">
                <ul>
                    <li v-for="(item1,index1) in items.children" :key="index1">
                        <a href="">
                            <i>{{index1+1}}</i>
                            <strong>{{item1.city}}</strong>
                            <span>{{item1.desc}}</span>
                        </a>
                    </li>
                </ul>
            </div>

        </div>
        <div class="aside-recommend">
            <h4 class="aside-title">推荐城市</h4>
            <a href="#" class="aside-recommend-item">
                <img src="http://157.122.54.189:9093/images/pic_sea.jpeg" >
            </a>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            menus:"",
            items:{},
            cityList:[],
            sublist:false,
            current:5,
            
            
            

        }
    },
    mounted(){
        this.$axios({
            url: `posts/cities`,
            }).then(res => {
            this.cityList = res.data.data;
            
               
        });
    },
    methods:{
        handleEnt(index,item){
            this.sublist=true;
            this.menus = index;
            this.items = item;
            this.current=index;
            
        },
        
    }

}
</script>

<style scoped lang="less">

.menus-wrapper{
    width: 260px;
    // background-color: #000;
    .menus-body{
        width: 260px;
        border: 1px solid #ddd;
        border-right: none;
        border-bottom: none;
        box-shadow: 0 0 1px #f5f5f5;
        
        .menu-item{
            height: 40px;
            line-height: 40px;
            border-bottom: 1px solid #ddd;
            border-right: 1px solid #ddd;
            padding: 0 20px;
            font-size: 14px;
            position: relative;
            &:after{
            display: block;
            content: "";
            width: 10px;
            height: 10px;
            border-right: 1px solid #999;
            border-top: 1px solid #999;
            -webkit-transform: rotate(45deg);
            transform: rotate(45deg);
            position: absolute;
            right: 20px;
            top: 15px;
            }
            
        }
        .active{
            border-right-color: #fff;
            color: orange;
            &:after{
             border-right-color: orange;
            border-top-color: orange;
            }
           
        }
        .sub-menus{
            z-index: 99;
            position: absolute;
            left: 260px;
            top: 0;
            width: 350px;
            padding: 10px 20px;
            box-sizing: border-box;
            background: #fff;
            border: 1px solid #ddd;
            ul{
                li{
                    font-size: 14px;
                    line-height: 1.5;
                    a{
                        i{
                            color: orange;
                            font-size: 24px;
                            font-style: italic;
                        }
                        strong{
                            margin: 0 10px;
                            color: orange;
                            font-weight: 400;
                        }
                        span{
                            color: #999;
                        }
                    }
                }
            }
        }
    }
    
    .aside-recommend{
        margin-top: 20px;
        .aside-title{
            font-weight: 400;
            padding-bottom: 10px;
            border-bottom: 1px solid #ddd;
            margin-bottom: 10px;
        }
        a{
            img{
                width: 100%;
                display: block;
            }
        }
    }
}
</style>
