<template>
    <div>
        <!-- 导航 -->
        <van-nav-bar style="font-size: 22px;padding:5px;" title="比芯">
        </van-nav-bar>
        <!--  tab标签页  -->
        <van-tabs sticky v-model="active">
            <van-tab title="笔芯">
                <div style="margin: 60px;">
                    <h1>剩余数量：<span>{{stationery.cartridge}}</span></h1>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showborrow">
                        <h3><span>借用数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="borrow('cartridge')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBorrowPopup" block native-type="submit" round size="normal" type="info"
                                v-if="stationery.cartridge!=0">借用
                    </van-button>
                    <br>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showback">
                        <h3><span>归还数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="giveback('cartridge')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBackPopup" block native-type="submit" round type="info">归还</van-button>
                </div>
            </van-tab>
            <van-tab title="橡皮擦">
                <div style="margin: 60px;">
                    <h1>剩余数量：<span>{{stationery.eraser}}</span></h1>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showborrow">
                        <h3><span>借用数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="borrow('eraser')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBorrowPopup" block native-type="submit" round size="normal" type="info"
                                v-if="stationery.eraser!=0">借用
                    </van-button>
                    <br>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showback">
                        <h3><span>归还数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="giveback('eraser')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBackPopup" block native-type="submit" round type="info">归还</van-button>
                </div>
            </van-tab>
            <van-tab title="草稿纸">
                <div style="margin: 60px;">
                    <h1>剩余数量：<span>{{stationery.paper}}</span></h1>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showborrow">
                        <h3><span>借用数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="borrow('paper')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBorrowPopup" block native-type="submit" round size="normal" type="info"
                                v-if="stationery.paper!=0">借用
                    </van-button>
                    <br>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showback">
                        <h3><span>归还数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="giveback('paper')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBackPopup" block native-type="submit" round type="info">归还</van-button>
                </div>
            </van-tab>
            <van-tab title="雨伞">
                <div style="margin: 60px;">
                    <h1>剩余数量：<span>{{stationery.umbrella}}</span></h1>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showborrow">
                        <h3><span>借用数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="borrow('umbrella')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBorrowPopup" block native-type="submit" round size="normal" type="info"
                                v-if="stationery.umbrella!=0">借用
                    </van-button>
                    <br>
                    <van-popup :style="{ height: '26%' }" position="bottom" round v-model="showback">
                        <h3><span>归还数量</span></h3>
                        <van-stepper @change="onChange" async-change button-size="32px" input-width="40px" max="5" min="1"
                                     theme="round" v-model="count"/>
                        <br>
                        <van-button @click="giveback('umbrella')" type="info">确定</van-button>
                    </van-popup>
                    <van-button @click="showBackPopup" block native-type="submit" round type="info">归还</van-button>
                </div>
            </van-tab>
        </van-tabs>
        <!--  底部导航栏  -->
        <van-tabbar placeholder route v-model="active">
            <van-tabbar-item icon="home-o" replace to="/home">首页</van-tabbar-item>
            <van-tabbar-item icon="user-o" replace to="/personalcenter">我的</van-tabbar-item>
        </van-tabbar>
    </div>
</template>

<script>
    import {
        Toast,
        PullRefresh,
        Swipe,
        SwipeItem
    } from 'vant';

    export default {
        comments: {},
        data() {
            return {
                stationery: '',
                showborrow: false,
                showback: false,
                count: '',
                active: '',
                loading: ''
            }
        },
        mounted() {
            if (localStorage.getItem("exsitUser") == null || localStorage.getItem("exsitUser") == '') {
                alert("请先登录")
                this.$router.push('/login')
            }
        },
        created() {
            const _this = this
            this.$axios.get('/api/getStationeryNumber').then(function (resp) {
                _this.stationery = resp.data[0] //前端在这里发送请求给后端，后端返回读取数据库的值
            })
        },
        methods: {
            onClick(index) {
                const _this = this
                this.$axios.get('http://localhost:8181/phone/findByCategoryType/' + this.categories[index].type).then(function (resp) {
                    _this.phones = resp.data.data
                })
            },
            onSearch(val) {
                Toast(val);
            },
            showMore(index) {
                // 路由跳转到http://localhost:8080/details?index
                this.$router.push({path: '/particulars', query: {id: index}})
            },
            onBuyClicked(item) {
                this.$store.state.specsId = item.selectedSkuComb.s1
                this.$store.state.quantity = item.selectedNum
                this.$router.push('/addressList')
            }, moneyOnChange(money) {
                Toast({
                    message: money[0] + " - " + money[1],
                    position: 'middle',
                });
            },
            borrow(type) {
                const _this = this
                console.log("借用的类型：" + type + _this.count)
                this.$axios({
                    url: '/api/borrow/' + type + '/' + _this.count + '/' + localStorage.getItem("exsitUser"),
                    method: 'post',
                    data: ''
                }).then(function (resp) {
                    alert(resp.data)
                    location.reload();
                })
            },
            giveback(type) {
                const _this = this
                console.log("归还的类型：" + type + _this.count)
                this.$axios({
                    url: '/api/giveback/' + type + '/' + _this.count + '/' + localStorage.getItem("exsitUser"),
                    method: 'post',
                    data: ''
                }).then(function (resp) {
                    alert(resp.data)
                    location.reload();
                })
            },
            onChange(value) {
                Toast.loading({forbidClick: true});
                clearTimeout(this.timer);
                this.timer = setTimeout(() => {
                    Toast.clear();
                    // 注意此时修改 value 后会再次触发 change 事件
                    this.count = value;
                }, 500);
            },

            showBorrowPopup() {
                this.showborrow = true;
            },
            showBackPopup() {
                this.showback = true;
            },
        }
    }
</script>


<style>
    .my-swipe .van-swipe-item {
        color: #fff;
        font-size: 20px;
        line-height: 10px;
        text-align: center;
        background-color: #ffffff;
    }

    .van-nav-bar__title {
        font-size: 25px !important;
    }

    .van-card__thumb img {
        border-radius: 2px !important;
        width: 150% !important;
    }
</style>
