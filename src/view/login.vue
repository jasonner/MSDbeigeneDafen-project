<template>
    <div class="loginPage" :style="{backgroundImage: 'url(' + basemapurl + ')', backgroundSize:'100% 100%'}">
        <!-- logo -->
        <div class="logo-box">
            <div class="header">
                <img :src="logourl" alt="" srcset="">
            </div>
        </div>
        <!-- slogan -->
        <div class="slogan-box">
            <div class="slogan">
                <img :src="sologonurl" alt="" srcset="">
            </div>
        </div>
        <!-- input -->
        <div class="input-box">
            <div class="input">
                <input type="text" v-model="userName" name="" maxlength="10" placeholder="请您填写姓名" id="">
            </div>
        </div>
        <!-- sure-submit -->
        <div class="sure-submit-box">
            <button @click="login()">
                <img src="@/assets/img/login/sureBtn.png" alt="" srcset="">
            </button>
        </div>
    </div>
</template>
<script>
import qs from 'qs'
import { Notify } from 'vant';
export default {
    name:'login',
    data(){
        return{
            userName:'',
            pid:'1',
            logourl:require('../assets/img/eg/logo.png'),
            sologonurl:require('../assets/img/eg/slogan.png?v=20220407'),
            basemapurl:require('../assets/img/eg/bg.png'),
        }
    },
    mounted(){
        var query=this.$route.query;
        this.pid = query.pid;
        this.initIslogin();
        this.getPageImgInfo();
    },
    methods:{
        //判断是否登录
        initIslogin(){  
            let that = this;
            let promise = {
                pid:that.pid
            }
            that.axios.post('msd1223_GetUserLogined', qs.stringify(promise))
            .then(function (res) {
                console.log(res.data)
                if(res.status && res.status ==200){
                    if(res.data.msg == "ok"){
                        that.$router.replace({path:'/index',query:{pid:that.pid}});
                    }else{
                       return
                    }
                }else{
                    that.$message({
                        message: '服务异常请稍后重试！',
                        type: 'warning'
                    });
                }
            })
        },

        //登录
        login(){
            let that  = this;
            if(that.userName== ''){
                Notify({ type: 'warning', message: '请输入您的姓名' });
            }else{
                let promise ={
                    username:that.userName,
                    pid:that.pid
                };
                that.axios.post('msd1223_Login', qs.stringify(promise))
                .then(function (res) {
                    console.log(res.data)
                    if(res.status && res.status ==200){
                        if(res.data.msg == "ok"){
                            that.$router.replace({path:'/index',query:{pid:that.pid}});
                        }else{
                            alert('该用户不存在，请联系工作人员')
                        }
                    }else{
                        Notify({ type: 'warning', message: '服务异常请稍后重试！'});
                    }
                })
            }
        },

        //获取页面图片设置信息
        getPageImgInfo(){
            let that = this;
            that.axios.get("msd1223_GetPById?pid=" + that.pid)
            .then(function (res) {
                console.log(res.data);
                if (res.status == 200) {
                if(res.data.msg && res.data.msg == 'ok'){
                    console.log(res.data.data.p)
                    if(res.data.data.p.logourl){
                        that.logourl = res.data.data.p.logourl;
                    };
                    if(res.data.data.p.sologonurl){
                        that.sologonurl = res.data.data.p.sologonurl;
                    };
                    if(res.data.data.p.basemapurl){
                        that.basemapurl = res.data.data.p.basemapurl;
                    };
                }else{
                    that.$message({
                    message:res.data.msg,
                    type: "warning",
                    });
                }
                } else {
                that.$message({
                    message: "服务异常请稍后重试！",
                    type: "warning",
                });
                }
            });
        },
    },
}
</script>
<style scoped>
    .loginPage{
        width: 100%;
        min-height: 100%;
        background: url('../assets/img/login/bg.png') center no-repeat;
        background-size: 100% 100%;
        box-sizing: border-box;
    }
    /* logo */
    .logo-box{
        width: 100%;
        display: flex;
    }
    .logo-box .header{
        width: 100%;
        margin: 0 auto;
        height: auto;
    }
    .logo-box .header img{
        width: 100%;
        object-fit: fill;
        margin-top: 42px;
    }
    /* slogan */
    .slogan-box{
        width: 100%;
    }
     /* TODO */
    /* .slogan-box .slogan{
        width: 87%;
        height: auto;
        margin: 0 auto;
    } */
   
    .slogan-box .slogan{
        width: 100%;
        height: auto;
        margin: 0 auto;
    }
    .slogan-box .slogan img{
        width: 100%;
        margin-top: 209px;
    }
    /* input */
    .input-box{
        width: 100%;
        margin-top: 153px;
    }
    .input-box .input{
        width: 74%;
        height: 114px;
        background: url('../assets/img/login/loginBg.png') center no-repeat;
        background-size: 100% 100%;
        margin: 0 auto;
    }
    .input-box .input input{
        width: 100%;
        height: 100%;
        border: none;
        background: transparent;
        font-size: 38px;
        font-family: Alibaba, Alibaba-PuHuiTi-R;
        font-weight: 700;
        text-align: center;
        color: #000;
        letter-spacing: 10px;
    }
    .input-box .input ::placeholder {
        color: #000;
        font-size: 38px;
    }
    /* sure-submit-box */
    .sure-submit-box{
        width: 100%;
        height: auto;
        margin-top: 38px;
    }
    .sure-submit-box button{
        width: 140px;
        height: 50px;
        margin: 0 auto;
        text-align: center;
        border: none;
        background: transparent;
    }
    .sure-submit-box button img{
        width: 100%;
    }
</style>