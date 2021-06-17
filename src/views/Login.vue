<template>
    <div>

        <van-tabs>
            <van-tab name="a" title="密码登录">
                <van-form @submit="onSubmit" style="margin-top:45%; margin-bottom:40%">
                    <van-field
                            :rules="[{ required: true, message: '请填写用户名' }]"
                            label="用户名"
                            name="username"
                            placeholder="请输入用户名"
                            v-model="username"
                    />
                    <van-field
                            :rules="[{ required: true, message: '请填写密码' }]"
                            label="密码"
                            name="password"
                            placeholder="请输入密码"
                            type="password"
                            v-model="password"
                    />
                    <div style="margin: 60px;">
                        <van-button block native-type="submit" round type="info">登录</van-button>
                        <br>
                        <van-button block round to="/register" type="info">前往注册</van-button>
                    </div>
                </van-form>
            </van-tab>
            <van-tab name="b" title="人脸登录">
                <div id="regcoDiv" style="margin-top:45%; margin-bottom:40%">
                    <video autoplay='autoplay' height='300px' id='video2' style='margin-top: 20px'
                           width='300px'></video>
                    <canvas height='500px' id='canvas2' style='display: none' width='500px'></canvas>
                    <van-button @click="getMedia2" title="人脸识别" value="人脸识别">开启摄像头</van-button>
                    <van-button @click="chooseFileChangeComp" id="snap" type="info">登录</van-button>
                    <div>
                        <img id="imageDivComp"/>
                    </div>
                </div>
            </van-tab>
        </van-tabs>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                username: '',
                password: '',
                base64File: '',
                result: '',
                similarValue: ''
            };
        },
        methods: {
            onSubmit(values) {
                const _this = this
                const formData = new FormData()
                formData.append('username', values.username)
                formData.append('password', values.password)
                this.$axios({
                    url: '/api/login',
                    method: 'post',
                    data: formData
                }).then(function (resp) {
                    console.log(resp.data.code);
                    console.log(resp.data.msg);

                    if (resp.data.code == "400") {
                        alert(resp.data.msg)
                    } else if (resp.data.code == "800") {
                        alert(resp.data.msg)
                    } else {
                        localStorage.setItem("exsitUser", resp.data.exsitUser);//存储登录记录
                        alert(resp.data.msg)
                        _this.$router.push('/home')
                    }

                })
            },

            getMedia2() {
                let constraints = {
                    video: {width: 500, height: 500},
                    audio: true
                };
                //获得video摄像头区域
                let video = document.getElementById("video2");
                let promise = navigator.mediaDevices.getUserMedia(constraints);
                promise.then(function (MediaStream) {
                    video.srcObject = MediaStream;
                    video.play();
                });
            },

            chooseFileChangeComp() {
                let _this = this;
                let regcoDivComp = document.getElementById("regcoDiv");
                let video = document.getElementById("video2");
                let canvas = document.getElementById("canvas2");
                let ctx = canvas.getContext('2d');
                ctx.drawImage(video, 0, 0, 500, 500);
                var formData = new FormData();
                formData.append("username", this.username)
                formData.append("groupId", "101")
                formData.append("file", canvas.toDataURL());
                this.$axios({
                    url: '/api/faceLogin',
                    method: 'post',
                    data: formData,
                }).then(function (resp) {
                    console.log(resp.data.code)
                    if (resp.data.code == 0) {
                        _this.result = resp.data.data
                        _this.similarValue = resp.data.data.similarValue
                        _this.username = resp.data.data.username
                        alert("登陆成功！匹配度：" + _this.similarValue + "，用户名：" + _this.username)
                        localStorage.setItem("exsitUser", _this.username);
                        _this.$router.push('/home')
                    } else if (resp.data.code == 14) {
                        alert("未检出到人脸")
                    } else if (resp.data.code == 15) {
                        alert("人脸不匹配")
                    }
                });

            }
        },
    };
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
