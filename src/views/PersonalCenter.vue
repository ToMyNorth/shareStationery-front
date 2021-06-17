<template>
  <div>
    <!-- 悬浮按钮 -->
    <div v-if="!isShow" class="xuanfu" id="moveDiv"
         @mousedown="down" @touchstart="down"
         @mousemove="move" @touchmove="move"
         @mouseup="end" @touchend="end"
    >
      <div class="yuanqiu" @click="logout">
        退出
      </div>
    </div>
    <van-nav-bar title="个人中心"></van-nav-bar>
    <!--  tab标签页  -->
    <van-tabs v-model="active" sticky>
      <van-tab @click="getBorrowRecord" title="我租用的">
        <div>
          <van-list
                  @load="getBorrowRecord"
          >
            <van-cell v-for="item in borrowList" :key="item" :title="item" />
          </van-list>
        </div>
      </van-tab>
      <van-tab  @click="getBackRecord" title="我归还的">
          <div>
            <van-list @click="getBackRecord"
                    @load="getBackRecord"
            >
              <van-cell v-for="item in backList" :key="item" :title="item" />
            </van-list>
        </div>
      </van-tab>
    </van-tabs>
    <!--  底部导航栏  -->
    <van-tabbar v-model="active" placeholder route>
        <van-tabbar-item replace to="/home" icon="home-o">首页</van-tabbar-item>
        <van-tabbar-item replace to="/personalcenter" icon="user-o">我的</van-tabbar-item>
      </van-tabbar>
  </div>

</template>

<script>
  import { Dialog } from 'vant';
  export default {
    data() {
      return {
        borrowList: [],
        backList:[],
        loading: false,
        finished: false,
        exsitUser:'',
        active:'',
        flags: false,
        position: { x: 0, y: 0 },
        nx: '', ny: '', dx: '', dy: '', xPum: '', yPum: '',
      };
    },
    mounted(){
      if (localStorage.getItem("exsitUser")==null||localStorage.getItem("exsitUser")==''){
        alert("请先登录")
        this.$router.push('/login')
      }
    },
    created(){

    },
    methods: {
      onload(){

      },
      getBackRecord(){
        const _this = this
        this.$axios({
          url: '/api/getBackRecord/'+localStorage.getItem("exsitUser"),
          method: 'get',
          data:''
        }).then(function (resp) {
          for (let i = 0; i < resp.data.length; i++) {
            if (resp.data[i].type=="eraser") {
              _this.backList.push(resp.data[i].time+"  类型：橡皮擦"+'  数量：'+resp.data[i].count);
            }
            if (resp.data[i].type=="cartridge") {
              _this.backList.push(resp.data[i].time+"  类型：笔芯"+'  数量：'+resp.data[i].count);
            }
            if (resp.data[i].type=="paper") {
              _this.backList.push(resp.data[i].time+"  类型：草稿纸"+'  数量：'+resp.data[i].count);
            }
            if (resp.data[i].type=="umbrella") {
              _this.backList.push(resp.data[i].time+"  类型：雨伞"+'  数量：'+resp.data[i].count);
            }
          }
          console.log("还的记录：",resp.data)
          // 加载状态结束
          _this.loading = false;
          // 数据全部加载完成
          if (this.backList.length >= resp.data.length) {
            _this.finished = true;
          }
        })
      },
      getBorrowRecord(){
        const _this = this
        this.$axios({
          url: '/api/getBorrowRecord/'+localStorage.getItem("exsitUser"),
          method: 'get',
          data:''
        }).then(function (resp) {
          for (let i = 0; i < resp.data.length; i++) {
            if (resp.data[i].type=="eraser") {
              _this.borrowList.push(resp.data[i].time+"  类型：橡皮擦"+'  数量：'+resp.data[i].count);
            }
            if (resp.data[i].type=="cartridge") {
              _this.borrowList.push(resp.data[i].time+"  类型：笔芯"+'  数量：'+resp.data[i].count);
            }
            if (resp.data[i].type=="paper") {
              _this.borrowList.push(resp.data[i].time+"  类型：草稿纸"+'  数量：'+resp.data[i].count);
            }
            if (resp.data[i].type=="umbrella") {
              _this.borrowList.push(resp.data[i].time+"  类型：雨伞"+'  数量：'+resp.data[i].count);
            }
          }
          console.log("借的记录：",resp.data)
          // 加载状态结束
          this.loading = false;
          // 数据全部加载完成
          if (this.borrowList.length >= resp.data.length) {
            this.finished = true;
          }
        })
      },

      // 实现移动端拖拽
      down(){
        this.flags = true;
        var touch;
        if(event.touches){
          touch = event.touches[0];
        }else {
          touch = event;
        }
        this.position.x = touch.clientX;
        this.position.y = touch.clientY;
        this.dx = moveDiv.offsetLeft;
        this.dy = moveDiv.offsetTop;
      },
      move(){
        if(this.flags){
          var touch ;
          if(event.touches){
            touch = event.touches[0];
          }else {
            touch = event;
          }
          this.nx = touch.clientX - this.position.x;
          this.ny = touch.clientY - this.position.y;
          this.xPum = this.dx+this.nx;
          this.yPum = this.dy+this.ny;
          moveDiv.style.left = this.xPum+"px";
          moveDiv.style.top = this.yPum +"px";
          //阻止页面的滑动默认事件；如果碰到滑动问题，1.2 请注意是否获取到 touchmove
          document.addEventListener("touchmove",function(){
            event.preventDefault();
          },false);
        }
      },
//鼠标释放时候的函数
      end(){
        this.flags = false;
      },

      logout(){
        Dialog.confirm({
          title: '退出登录',
          message: '您确定要退出当前账户吗？',
        })
        .then(() => {
          // on confirm
          localStorage.clear(); //清空登录记录，清空本地的localstorage
          this.$router.push('/login')
        })
        .catch(() => {
          // on cancel
        });
      }
    },
  };
</script>

<style scoped>
  .xuanfu {
    height: 0rem;
    width: 0rem;
    /* 如果碰到滑动问题，1.3 请检查 z-index。z-index需比web大一级*/
    z-index: 999;
    position: fixed;
    top: 545.133px;
    left:309.933px;
    right: 3.2rem;
    border-radius: 0.8rem;
    background-color: rgba(0, 0, 0, 0.55);
  }
  .yuanqiu {
    height: 3.0rem;
    width: 3.0rem;
    border: 0.3rem solid rgba(140, 136, 136, 0.5);
    margin: 0.65rem auto;
    color: #646566;
    font-size: 1.0rem;
    line-height: 2.7rem;
    text-align: center;
    border-radius: 100%;
    background-color: #ffffff;
  }
</style>
