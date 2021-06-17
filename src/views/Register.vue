<template>
    <div id="app">
        <van-nav-bar title="注册账号"  fixed = true />
        <van-form style="margin-top:15%; margin-bottom:15%">
            <van-field
                    v-model="username"
                    name="username"
                    label="用户名"
                    placeholder="用户名"
                    :rules="[{ required: true, message: '请填写用户名' }]"
            />
            <van-field
                    v-model="password"
                    name="password"
                    label="密码"
                    type="password"
                    placeholder="登录密码"
                    :rules="[{ required: true, message: '请填写登录密码' }]"
            />
            <van-field
                    v-model="studentnumber"
                    name="studentnumber"
                    label="学号"
                    placeholder="学号"
                    :rules="[{ required: true, message: '请填写学号' }]"
            />
            <van-field
                    v-model="academe"
                    name="academe"
                    label="所在学院"
                    placeholder="所在学院"
                    :rules="[{ required: true, message: '请填写所在学院' }]"
            />
            <van-field
                    v-model="realname"
                    name="realname"
                    label="真实姓名"
                    placeholder="真实姓名"
                    :rules="[{ required: true, message: '请填写真实姓名' }]"
            />
            <div id="mainDiv">
                <video id='video' width='300px' height='300px' autoplay='autoplay' style='margin-top: 20px'></video><canvas id='canvas' width='500px' height='500px' style='display: none'></canvas>
            </div>
            <van-button @click="getMedia" title="摄像头注册" value="摄像头注册">开启摄像头</van-button>
            <van-button id="snap" @click="takePhoto" type="info">获取人脸信息</van-button>
        </van-form>
            <div style="margin: 60px;">
                <van-button type="info" @click="onSubmit" block round style="margin-top: 20px;" >注册账户</van-button>
                <br>
                <van-button block round to="/login" type="info">前往登录</van-button>
            </div>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                username: '',
                password: '',
                academe: '',
                realname: '',
                studentnumber: '',
                srcObject:'',
                base64File:''
            };
        },
        methods: {
            onSubmit(values) {
                const _this = this
                const formData = new FormData()
                formData.append('username',this.username)
                formData.append('password', this.password)
                formData.append('academe', this.academe)
                formData.append('realname',this.realname)
                formData.append('studentNumber', this.studentnumber)
                formData.append("groupId", "101")
                formData.append("file",this.base64File)
                this.$axios({
                    url: '/api/register',
                    method: 'post',
                    data: formData
                }).then(function (resp) {
                    alert(resp.data.msg)
                    _this.$router.push('/login')
                })
            },

            getMedia() {
                let videoComp = "";
                document.getElementById("mainDiv").append(videoComp);
                let constraints = {
                    video: {width: 500, height: 500},
                    audio: true
                };
                let video = document.getElementById("video");
                let promise = navigator.mediaDevices.getUserMedia(constraints);
                promise.then(function (MediaStream) {
                    video.srcObject = MediaStream;
                    video.play();
                });
            },

            takePhoto() {
                let _this = this;
                let mainComp = document.getElementById("mainDiv");
                    //获得Canvas对象
                    let video = document.getElementById("video");
                    let canvas = document.getElementById("canvas");
                    console.log(canvas)
                    let ctx = canvas.getContext('2d');
                    ctx.drawImage(video, 0, 0, 500, 500);
                    var formData = new FormData();
                    var base64File = canvas.toDataURL();
                    _this.base64File = base64File
                        alert("获取人脸信息成功，请确认信息无误后提交注册")
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

    .van-grid-item__text {
        color: #646566;
        font-size: 15px !important;
        line-height: 3.5 !important;
        word-break: break-all;
    }
</style>
