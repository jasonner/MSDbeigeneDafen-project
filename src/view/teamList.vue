<template>
    <div class="teamListPage" :style="{backgroundImage: 'url(' + basemapurl + ')', backgroundSize:'100% 100%'}">
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
      <!-- Leaderboard -->
      <div class="leaderboard-box">
          <div class="leaderboard">
            <div class="leaderboard-list">
              <div>排名</div>
              <div>参赛团队</div>
              <div>得分</div>
            </div>
            <div class="leaderboard-list" v-for="(item,index) in teamList" :key="index">
              <div :class="{active:(index==0||index==1)}"><span>{{index+1}}</span></div>
              <div>{{item.hospital}}</div>
              <div v-if="item.setscore>0">{{item.setscore}}</div>
              <div v-else>{{item.score}}</div>
            </div>
          </div>
      </div>

      <div class="back-box" @click="backChange()">
        <img src="@/assets/img/back.png" alt="" srcset="">
      </div>

      
    </div>  
</template>
<script>
import Vue from "vue";
import { Notify } from "vant";
Vue.use(Notify);
export default {
    data(){
        return{
          pid:'',
          teamList:[{
            index:1,
            content:'广州医科大学附属第一医院',
            score:'90'
          },{
            index:2,
            content:'中国人民解放军总医院第一医学中心',
            score:'90'
          },{
            index:3,
            content:'中国人民解放军总医院第一医学中心',
            score:'90'
          },{
            index:4,
            content:'广州医科大学附属第一医院',
            score:'90'
          },{
            index:5,
            content:'中国人民解放军总医院第一医学中心',
            score:'90'
          },{
            index:6,
            content:'广州医科大学附属第一医院',
            score:'90'
          }],
          logourl:require('../assets/img/eg/logo.png'),
          sologonurl:require('../assets/img/eg/slogan.png?v=20220407'),
          basemapurl:require('../assets/img/eg/bg.png'),
        }
    },
    mounted(){
      var query = this.$route.query;
      if (query.pid && query.pid != '') {
        this.pid = query.pid;
        this.historyFun();
        this.getPageImgInfo();
      }else{
        this.$router.push({path:'/login'});
      }
    },
    methods:{
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
        //获取历史打分
        historyFun() {
            let that = this;
            console.log(that.changci);
            that.axios.get("msd1223_GetRankList?pid=" + that.pid).then(function (res) {
                console.log(res);
                if (res.status == 200) {
                  if(res.data.msg && res.data.msg =='ok'){
                    if(res.data.data &&res.data.data.list && res.data.data.list.length > 0){
                      that.teamList = res.data.data.list;
                    }else{
                      Notify({
                            message: "暂无打分记录",
                            color: "#fff",
                            background: "#e72876",
                      });
                      that.teamList = [];
                    }
                  }else{
                    that.$message.error(res.data.msg);
                  }
                } else {
                        that.$message({
                        message: "服务异常请稍后重试！",
                        type: "warning",
                        });
                }
            });
        },

        //返回
        backChange(){
          this.$router.replace({path:'/index',query:{pid:this.pid}});
        }
    },

    destroyed() {

    },
}
</script>
<style scoped>
  .teamListPage{
      width: 100%;
      min-height: 100%;
      background-size: 100% 100%;
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
    margin-top: 56px;
  } 
  /* TODO */
  /* .slogan-box .slogan{
      width: 87%;
      height: auto;
      margin: 0 auto;
  } */
  .slogan-box .slogan{
      width: 74.04%;
      height: auto;
      margin: 0 auto;
  }
  .slogan-box .slogan img{
    width: 100%;
  }

  /* leaderboard-box */
  .leaderboard-box{
    width: 100%;
    height: 708px;
    overflow: scroll;
    margin-top: 62px;

  }
  .leaderboard-box .leaderboard{
    width: 80%;
    margin: 0 auto;
    height: auto;
  }
  .leaderboard-box .leaderboard .leaderboard-list{
    display: grid;
    grid-template-columns: 72px 494px 50px;
    margin-top: 47px;
  }
  .leaderboard-box .leaderboard .leaderboard-list div{
    text-align: left;
    font-family: AlibabaPuHuiTi, AlibabaPuHuiTi-Bold;
    font-weight: 700;
    color: #006261;
    font-size: 24px;
  }
  .leaderboard-box .leaderboard .leaderboard-list div:nth-of-type(1){
    text-align: center;
  }
  .leaderboard-box .leaderboard .leaderboard-list div:nth-last-of-type(1){
    text-align: center;
  }
  .leaderboard-box .leaderboard .leaderboard-list .active span{
    background: orange;
    display: inline-block;
    padding:8px 10px;
    box-sizing: border-box;
    margin-left: -8px;
  }
  .back-box{
    width: 10vw;
    position: fixed;
    bottom: 6vw;
    left: 6vw;
  }
  .back-box img{
    width: 100%;
    z-index: 1000;
  }
</style>