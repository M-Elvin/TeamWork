<template>
    <div class="flight-item">
        <div @click="handleShow">
            <!-- 显示的机票信息 -->
            <el-row type="flex" align="middle" class="flight-info">
                <el-col :span="6">
                    <span>{{ data.airline_name }} </span> {{data.flight_no}}
                </el-col>
                <el-col :span="12">
                    <el-row type="flex" justify="space-between" class="flight-info-center">
                        <el-col :span="8" class="flight-airport">
                            <strong>{{ data.dep_time }}</strong>
                            <span>{{ data.org_airport_name }}</span>
                        </el-col>
                        <el-col :span="8" class="flight-time">
                            <span>{{rankTime}}</span>
                        </el-col>
                        <el-col :span="8" class="flight-airport">
                            <strong>{{ data.arr_time }}</strong>
                            <span>{{ data.dst_airport_name }}</span>
                        </el-col>
                    </el-row>
                </el-col>
                <el-col :span="6" class="flight-info-right">
                    ￥<span class="sell-price">{{data.base_price / 2}}</span>起
                </el-col>
            </el-row>
        </div>
        <div class="flight-recommend" v-if="isShow">
            <!-- 隐藏的座位信息列表 -->
            <el-row type="flex"  justify="space-between" align="middle">
                <el-col :span="4">低价推荐</el-col>
                <el-col :span="20">

                    <!-- 座位的信息 -->
                    <el-row 
                    type="flex" 
                    justify="space-between" 
                    align="middle" 
                    class="flight-sell"
                    v-for="(item, index) in data.seat_infos"
                    :key="index">
                        <el-col :span="16" class="flight-sell-left">
                            <span>{{item.name}}</span> | 
                            {{item.supplierName}}
                        </el-col>
                        <el-col :span="5" class="price">
                            ￥{{item.org_settle_price}}
                        </el-col>
                        <el-col :span="3" class="choose-button">
                            <el-button 
                            type="warning" 
                            size="mini"
                            @click="handleToOrder(item)">
                            选定
                            </el-button>
                            <p>剩余：{{item.discount}}</p>
                        </el-col>
                    </el-row>

                </el-col>
            </el-row>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            isShow: false // 控制座位信息的显示隐藏
        }
    },
    
    props: {
        // 数据
        data: {
            type: Object,
            // 默认是空对象
            default: {}
        }
    },

    computed: {
        rankTime(){
            // 出发时间,到达时间
            const { dep_time, arr_time } = this.data;

            let arr = arr_time.split(":");
            const dep = dep_time.split(":");

            // 如果到达时间小于出发时间 - 就是第二天的时间
            if(arr[0] < dep[0]){
                arr[0] += 24;
            }

            const count = (arr[0] * 60 + +arr[1]) - (dep[0] * 60 + +dep[1]);

            const hours = Math.floor(count / 60);
            const min = count % 60;

            return `${hours}时${min}分钟`;
        }
    },

    methods: {
        // 显示隐藏座位信息
        handleShow(){
            this.isShow = !this.isShow;
        },

        // 跳转到订单
        handleToOrder(item){
            this.$router.push({
                // 跳转的页面路径
                path: "/air/order",
                query: {
                    id: this.data.id,
                    seat_xid: item.seat_xid
                }
            })
        }
    }
}
</script>

<style scoped lang="less">
    .flight-item{
        border:1px #ddd solid;
        margin-bottom: 10px;

        .flight-info{
            padding:15px;
            cursor: pointer;

            > div{
                &:first-child, &:last-child{
                    text-align: center;
                }
            }
        }

        .flight-info-center{
            padding:0 30px;
            text-align: center;

            .flight-airport{
                strong{
                    display: block;
                    font-size:24px;
                    font-weight: normal;
                }
                span{
                    font-size: 12px;
                    color:#999;
                }
            }

            .flight-time{
                span{
                    display: inline-block;
                    padding:10px 0;
                    border-bottom: 1px #eee solid;
                    color:#999;
                }
            }
        }

        .flight-info-right{
            
            .sell-price{
                font-size: 24px;
                color:orange;
                margin:0 2px;
            }
        }
    }

    .flight-recommend{
        background: #f6f6f6;
        border-top:1px #eee solid;
        padding:0 20px;

        .flight-sell{
            border-bottom:1px #eee solid;
            padding:10px 0;

            &:last-child{
                border-bottom: none;
            }

            .flight-sell-left{
                font-size: 12px;
                span{
                    color:green;
                }
            } 

            .price{
                font-size: 20px;
                color:orange;
            }

            .choose-button{
                text-align: center;
                color:#666;
                button{
                    display: block;
                    width:100%;
                    margin-bottom:5px;
                }
            }
        }
    }
</style>