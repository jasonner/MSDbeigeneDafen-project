<template>
  <div class="indexPage">
    <div class="startPage">
      <div class="header-list">
          <img :src="logourl" alt="" srcset="">
      </div>

      <!-- slogan -->
      <div class="slogan-box">
        <div>
           <img :src="sologonurl" alt="" srcset="">
        </div>
      </div>

      <!-- title -->
      <div class="text-login-box">
        <div>
          <img src="@/assets/img/index/title.png"/>
        </div>
      </div>

      <!-- select-box -->
      <div class="select-box"> 
          <div class="select-list" id="selectList">
              <img src="@/assets/img/index/selectTip.png" alt="" srcset="">
          </div>
      </div>

      <!-- select-team -->
      <div class="select-team-box">
          <div class="select-team-content">
            <van-radio-group v-model="radio">
              <div class="list" v-for="(item,index) in teamList" :key="index">
                <div class="list-item1"><van-radio :name="item.id" shape="square" @click="checkedChange(item)">{{item.hospital}}</van-radio></div>
                <div class="list-item2">
                  <button v-if="item.zijiscore>0">得分:{{item.zijiscore}}</button>
                  <button v-else>得分:0</button>
                </div>
              </div>
            </van-radio-group>
          </div>
      </div>

      <!-- 点击查看排名 -->
      <!-- <div class="Click-to-view-ranking-box">
          <div class="list" @click="lookRanking()">
            <img src="@/assets/img/index/lookAttach.png" alt="" srcset="">
          </div>
      </div>   -->

      <!-- 评分提示 -->
      <div class="rank-tip-box">
        <div>
          <img src="@/assets/img/index/lookAttachTip.png?v=20220514" alt="" srcset="">
        </div>
      </div>

      <!-- 评分表 -->
      <div class="score-title-box" v-show="teamTitle !=''">
        <div>
          <img src="@/assets/img/index/pingfenbiao.png" alt="" srcset="">
        </div>
      </div>
      
      <!-- 当前参赛队伍 -->
      <div class="current-participating-teams-box" v-show="teamTitle !=''"> 
        <div class="participating-teams-title">
          当前参赛队伍:<span>{{teamTitle}}</span>
        </div>
      </div>
       
      <!-- 评分标准 1-->
      <div class="score-content score-content1" v-show="teamTitle !=''">
        <div class="title">
          病例编撰（30分）
        </div>
        <div v-for="(item, index) in coreList1" :key="index">
          <div class="list">
            <div class="list-title">
              {{ item.title }}
            </div>
          </div>

          <!-- 滑杆 -->
          <div class="slide-bar-content">
            <!-- <span class="valueNum">{{ item.value }}</span> -->
            <div class="small-num">
              <p>0</p>
              <p>最小值</p>
            </div>
            <div class="slide-bar" v-if="index===0 || index===3">
              <van-slider
                @change="sliderChange1(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="7"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="slide-bar" v-else>
              <van-slider
                @change="sliderChange1(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="8"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="max-num" v-if="index===0 || index===3">
              <p>7</p>
              <p>最大值</p>
            </div>
            <div class="max-num" v-else>
              <p>8</p>
              <p>最大值</p>
            </div>
          </div>
        </div>
      </div>

      <!-- 评分标准 2-->
      <div class="score-content score-content2" v-show="teamTitle !=''">
        <div class="title">
          手术视频（25分）
        </div>
        <div v-for="(item, index) in coreList2" :key="index">
          <div class="list">
            <div class="list-title">
              {{ item.title }}
            </div>
          </div>
          <!-- 滑杆 -->
          <div class="slide-bar-content">
            <!-- <span class="valueNum">{{ item.value }}</span> -->
            <div class="small-num">
              <p>0</p>
              <p>最小值</p>
            </div>
            <div class="slide-bar" v-if="index===0">
              <van-slider
                @change="sliderChange2(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="15"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="slide-bar" v-else>
              <van-slider
                @change="sliderChange2(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="10"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="max-num" v-if="index===0">
              <p>15</p>
              <p>最大值</p>
            </div>
            <div class="max-num" v-else>
              <p>10</p>
              <p>最大值</p>
            </div>
          </div>
        </div>
      </div>

      <!-- 评分标准 3-->
      <div class="score-content score-content3" v-show="teamTitle !=''">
        <div class="title">
          病例演讲（15分）
        </div>
        <div v-for="(item, index) in coreList3" :key="index">
          <div class="list">
            <div class="list-title">
              {{ item.title }}
            </div>
          </div>
          <!-- 滑杆 -->
          <div class="slide-bar-content" >
            <!-- <span class="valueNum">{{ item.value }}</span> -->
            <div class="small-num">
              <p>0</p>
              <p>最小值</p>
            </div>
            <div class="slide-bar"  v-if="index===0">
              <van-slider
                @change="sliderChange3(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="8"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="max-num" v-if="index===0">
              <p>8</p>
              <p>最大值</p>
            </div>
            <div class="slide-bar"  v-if="index===1">
              <van-slider
                @change="sliderChange3(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="7"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="max-num" v-if="index===1">
              <p>7</p>
              <p>最大值</p>
            </div>
          </div>
        </div>
      </div>

       <!-- 评分标准 4-->
      <div class="score-content score-content3" v-show="teamTitle !=''">
        <div class="title">
         问题问答（30分）
        </div>
        <div v-for="(item, index) in coreList4" :key="index">
          <div class="list">
            <div class="list-title">
              {{ item.title }}
            </div>
          </div>
          <!-- 滑杆 -->
          <div class="slide-bar-content" >
            <!-- <span class="valueNum">{{ item.value }}</span> -->
            <div class="small-num">
              <p>0</p>
              <p>最小值</p>
            </div>
            <div class="slide-bar"  v-if="index===0">
              <van-slider
                @change="sliderChange4(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="20"
                :disabled="true"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="max-num" v-if="index===0">
              <p>20</p>
              <p>最大值</p>
            </div>
            <div class="slide-bar"  v-if="index===1">
              <van-slider
                @change="sliderChange4(item.value, index)"
                v-model="item.value"
                active-color="#007573"
                style="border: 0.1em solid #fff; background: #ffad0c"
                class="slide"
                :min="0"
                :max="10"
              >
                <template #button>
                  <div class="custom-button">{{ item.value }}</div>
                </template>
              </van-slider>
            </div>
            <div class="max-num" v-if="index===1">
              <p>10</p>
              <p>最大值</p>
            </div>
          </div>
        </div>
      </div>
      <!-- 得分总和 -->
      <div class="sum-of-scores-box" v-show="teamTitle !=''">
        <div class="title">
          参赛团队  总分{{TotalScore}}
        </div>
        <div class="content-btn" @click="subMitBtn()">
          <img src="@/assets/img/index/submitBtn.png" alt="" srcset="">
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import qs from "qs";
import Vue from "vue";
import { Slider } from "vant";
import { Swipe, SwipeItem } from "vant";
import { Notify } from "vant";
import { Dialog } from 'vant';
import { RadioGroup, Radio } from 'vant';

Vue.use(Radio);
Vue.use(RadioGroup);
Vue.use(Slider);
Vue.use(Swipe);
Vue.use(SwipeItem);
Vue.use(Notify);
Vue.use(Dialog);
export default {
  data() {
    return {
      teamList:[],
      radio:'',
      teamResult:[],
      coreList1: [
        {
          title: "1、质量：病例内容是否完整规范，及对病例理解是否透彻（7分）",
          value: 0,
        },
        {
          title: "2、价值：病例是否具有对临床有借鉴指导意义的观点或洞察（8分）",
          value: 0,
        },{
          title: "3、分析：诊疗思路、鉴别诊断是否准确清晰；是否结合合适文献和询证证据（8分）",
          value: 0,
        },
        {
          title: "4、幻灯：是否逻辑清晰、文字精炼、重点突出、图文并茂（7分）",
          value: 0,
        }
      ],
      coreList2: [
        {
          title: "1、技术：术式是否合理、操作规范；是否具有技术难度、体现中心特色（15分）",
          value: 0,
        },
        {
          title: "2、质量：视频资料是否完整、清晰；剪辑是否速度、时长合理、观感流畅（10分）",
          value: 0,
        }
      ],

      coreList3: [
        {
          title: "1、表达：语言表达是否清晰流畅、逻辑严密、具有感染力、是否使用标准规范的专业术语（8分）",
          value: 0,
        },
        {
          title: "2、时间：是否严格掌握演讲时间，充分利用时间且不超时（7分）",
          value: 0,
        }
      ],

      coreList4: [
        {
          title: "1、客观题：答题系统得分自动判定（20分）",
          value: 0,
        },
        {
          title: "2、主观题：回答评委提问是否敏捷自如、信息准确、具有一定的学术专业性（10分）",
          value: 0,
        }
      ],
      teamTitle: "",
      timer1: null,
      pid: "",
      TotalScore: 0,
      historyShow: false,
      TotalScoreList: {
        a1: 0,
        a2: 0,
        a3: 0,
        a4: 0,
        a5: 0,
        a6: 0,
        a7: 0,
        a8: 0,
        a9:0,
        a10:0,
        pid: "1",
        xuanshouid : "",
      },
      ranks: {
        id:'',
        daoshi:'',
        userName:''
      }, //代表队伍
      logourl:require('../assets/img/eg/logo.png'),
      sologonurl:require('../assets/img/eg/slogan.png?v=20220407'),
      basemapurl:require('../assets/img/index/bg.png'),
    };
  },
  mounted() {
    var query = this.$route.query;
    if (query.pid && query.pid != "") {
      this.pid = query.pid;
      this.getUserXuanshou();
      this.getScore();
      this.getPageImgInfo();
    }else{
      this.$router.push({path:'/login'});
    }
   
  },
  methods: {
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
    //获取选手信息
    getUserXuanshou() {
      let that = this;
      that.axios.get("msd1223_GetXuanshouList?pid=" + that.pid)
      .then(function (res) {
        console.log(res.data);
        if (res.status == 200) {
          if(res.data.msg && res.data.msg == 'ok'){
            if(res.data.data && res.data.data.xuanshoulist && res.data.data.xuanshoulist.length>0){
              that.teamList = res.data.data.xuanshoulist;
              res.data.data.xuanshoulist.forEach(element => {
                console.log(element.id ==that.TotalScoreList.xuanshouid)
                if(element.id ==that.TotalScoreList.xuanshouid){
                  that.coreList4[0].value = element.keguantiscore;
                  that.TotalScoreList.a9 = element.keguantiscore;
                  that.getScore();
                }
              });
            }else{
              that.teamList = [];
            }
            if(that.timer1){
              clearInterval(that.timer1);
            }
            that.timer1 = setTimeout(() => {
              that.getUserXuanshou();
            }, 3000);
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

    //滑杆取值1
    sliderChange1(val, index) {
      console.log(index);
      console.log(val);
      switch (index) {
        case 0:
          this.TotalScoreList.a1 = val;
          break;
        case 1:
          this.TotalScoreList.a2 = val;
          break;
        case 2:
          this.TotalScoreList.a3 = val;
          break;
        case 3:
          this.TotalScoreList.a4 = val;
          break;
        default:
          break;
      }
      this.getScore();
    },

    //滑杆取值2
    sliderChange2(val, index) {
      console.log(index);
      console.log(val);
      switch (index) {
        case 0:
          this.TotalScoreList.a5 = val;
          break;
        case 1:
          this.TotalScoreList.a6 = val;
          break;
        default:
          break;
      }
      this.getScore();
    },

    //滑杆取值3
    sliderChange3(val, index) {
      console.log(index);
      console.log(val);
      switch (index) {
        case 0:
          this.TotalScoreList.a7 = val;
          break;
        case 1:
          this.TotalScoreList.a8 = val;
          break;
        default:
          break;
      }
      this.getScore();
    },
    
    //滑杆取值4
    sliderChange4(val, index) {
      console.log(index);
      console.log(val);
      console.log('哈哈哈哈')
      switch (index) {
        case 0:
          this.TotalScoreList.a9 = val;
          break;
        case 1:
          this.TotalScoreList.a10 = val;
          break;
        default:
          break;
      }
      this.getScore();
    },

    //获取得分总和
    getScore() {
      let a1 = Number(this.TotalScoreList.a1);
      let a2 = Number(this.TotalScoreList.a2);
      let a3 = Number(this.TotalScoreList.a3);
      let a4 = Number(this.TotalScoreList.a4);
      let a5 = Number(this.TotalScoreList.a5);
      let a6 = Number(this.TotalScoreList.a6);
      let a7 = Number(this.TotalScoreList.a7);
      let a8 = Number(this.TotalScoreList.a8);
      let a9 = Number(this.TotalScoreList.a9);
      let a10 = Number(this.TotalScoreList.a10);
      this.TotalScore = a1 + a2 + a3 + a4 + a5 + a6 + a7 + a8 + a9 + a10;
    },

    //提交
    subMitBtn() {
      this.TotalScoreList.pid = this.pid;
      //this.TotalScoreList.xuanshouid  = this.ranks.id;
      let scorelist = this.TotalScoreList.a1+','+this.TotalScoreList.a2+','+this.TotalScoreList.a3+','+this.TotalScoreList.a4+','+this.TotalScoreList.a5+','+this.TotalScoreList.a6+','+this.TotalScoreList.a7+','+this.TotalScoreList.a8+','+this.TotalScoreList.a9+','+this.TotalScoreList.a10;
      let promise ={
        xuanshouid:this.TotalScoreList.xuanshouid,//选手ID
        scorelist:scorelist,//每项分数
        logstr:JSON.stringify({a1:this.TotalScoreList.a1,a2:this.TotalScoreList.a2,a3:this.TotalScoreList.a3,a4:this.TotalScoreList.a4,a5:this.TotalScoreList.a5,a6:this.TotalScoreList.a6,a7:this.TotalScoreList.a7,a8:this.TotalScoreList.a8,a9:this.TotalScoreList.a9,a10:this.TotalScoreList.a10}) ,//每项分数
        score:this.TotalScore,//总分
        pid:this.pid//场次 
      }
      let that = this;
      that.axios.post("msd1223_AddScore", qs.stringify(promise))
        .then(function (res) {
          console.log(res);
          if (res.status == 200) {
            if (res.data.msg && res.data.msg == 'ok') {
              Dialog.alert({
                message: '提交成功',
              }).then(() => {
                that.getUserXuanshou();
                that.$nextTick(()=>{
                  document.getElementById('selectList').scrollIntoView();
                })
              });
            } else {
              Notify({
                message: '投票通道暂未开启请稍后!',
                color: "#fff",
                background: "red",
              });
            }
          } else {
            Notify({
              message: "服务异常请稍后重试！",
              color: "#fff",
              background: "red",
            });
          }
        });
    },

    //查看排行榜
    lookRanking(){
      let that = this;
      that.axios.get("msd1223_GetStateByPid?pid="+this.pid)
      .then(function (res) {
        console.log(res);
        if (res.status == 200) {
          if (res.data && res.data.msg == 'ok') {
            if(res.data.data.state === 0){
              that.$router.replace({path:'/teamList',query:{pid:that.pid}});
            }else{
              Notify({
                message: "打分通道暂未关闭,请稍后！",
                color: "#fff",
                background: "red",
              });
            }
          } else {
            Notify({
              message: res.data.msg,
              color: "#fff",
              background: "red",
            });
          }
        } else {
          Notify({
            message: "服务异常请稍后重试！",
            color: "#fff",
            background: "red",
          });
        }
      });
    },

    //队伍切换
    checkedChange(row){
      console.log(row);
      this.teamTitle = row.hospital;
      this.TotalScoreList.xuanshouid = row.id;
      this.pid = row.pid;
      this.coreList4[0].value = row.keguantiscore;
      this.TotalScoreList.a9 = row.keguantiscore;
      this.getLogstr();
    },

    //获取回显
    getLogstr(){
      let that = this;
      that.axios.get("msd1223_Getlogstr?pid="+this.pid+'&xuanshouid='+this.TotalScoreList.xuanshouid)
      .then(function (res) {
        console.log(res.data);
        if (res.status == 200) {
          if (res.data && res.data.msg == 'ok') {
            if(res.data.data && res.data.data.logstr){
              let logstr = JSON.parse(res.data.data.logstr);
              for(var i=0; i<that.coreList1.length;i++){
                that.coreList1[0].value = logstr.a1;
                that.coreList1[1].value = logstr.a2;
                that.coreList1[2].value = logstr.a3;
                that.coreList1[3].value = logstr.a4;
              }
              for(var j=0; j<that.coreList2.length;j++){
                that.coreList2[0].value = logstr.a5;
                that.coreList2[1].value = logstr.a6;
              }
              for(var k=0; k<that.coreList3.length;k++){
                that.coreList3[0].value = logstr.a7;
                that.coreList3[1].value = logstr.a8;
              }
              for(var m=0; m<that.coreList4.length;m++){
                that.coreList4[0].value = logstr.a9;
                that.coreList4[1].value = logstr.a10;
              }
              that.TotalScoreList.a1= logstr.a1;
              that.TotalScoreList.a2= logstr.a2;
              that.TotalScoreList.a3= logstr.a3;
              that.TotalScoreList.a4= logstr.a4;
              that.TotalScoreList.a5= logstr.a5;
              that.TotalScoreList.a6= logstr.a6;
              that.TotalScoreList.a7= logstr.a7;
              that.TotalScoreList.a8= logstr.a8;
              that.TotalScoreList.a10= logstr.a10;
              that.getScore();
            }else{
              for(var i=0; i<that.coreList1.length;i++){
                that.coreList1[0].value = 0;
                that.coreList1[1].value = 0;
                that.coreList1[2].value = 0;
                that.coreList1[3].value = 0;
              }
              for(var j=0; j<that.coreList2.length;j++){
                that.coreList2[0].value = 0;
                that.coreList2[1].value = 0;
              }
              for(var k=0; k<that.coreList3.length;k++){
                that.coreList3[0].value = 0;
                that.coreList3[1].value = 0;
              }
               for(var m=0; m<that.coreList4.length;m++){
                that.coreList4[1].value = 0;
              }
              that.TotalScoreList.a1= 0;
              that.TotalScoreList.a2= 0;
              that.TotalScoreList.a3= 0;
              that.TotalScoreList.a4= 0;
              that.TotalScoreList.a5= 0;
              that.TotalScoreList.a6= 0;
              that.TotalScoreList.a7= 0;
              that.TotalScoreList.a8= 0;
              that.TotalScoreList.a10= 0;
              that.getScore();
            }
            
          } else {
            Notify({
              message: res.data.msg,
              color: "#fff",
              background: "red",
            });
          }
        } else {
          Notify({
            message: "服务异常请稍后重试！",
            color: "#fff",
            background: "red",
          });
        }
      });
    }
  },

  destroyed() {
    if (this.timer1) {
      clearInterval(this.timer1);
    }
  },
};
</script>
<style>
.select-team-content .van-checkbox__icon .van-icon{
    display: block;
    box-sizing: border-box;
    width: 2.25em;
    height: 2.25em;
    font-size: .8em;
    line-height: 1.5;
    text-align: center;
    border: 0.013333rem solid #1b9f9e;
}
.select-team-content .van-checkbox{
  height: 100%;
}
.select-team-content .van-checkbox__icon{
  height: 100%;
}
.select-team-content .van-radio__icon .van-icon{
  font-size: 4vw !important;
  line-height: 1;
  width: 4vw;
  height: 4vw;
}
.select-team-content .van-radio__icon{
  height: 4vw;

}
.select-team-content .van-radio__label{
  font-size:24px;
}
.select-team-content  .van-radio__label{
  line-height: normal;
}
</style>
<style scoped>
.indexPage {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
}
.noStart {
  width: 100%;
  min-height: 100%;
  background: url("../assets/img/index/bg.png") center no-repeat;
  background-size: 100% 100%;
}
.startPage{
  width: 100%;
  min-height: 100%;
  background: url("../assets/img/index/bg.png") center no-repeat;
  background-size: 100% 100%;
}
.indexPage .page1 {
  width: 100%;
  /* background: url("https://oss.sxyweb.com.cn/2021/wj/BeiGene/indexBg.png")
    center no-repeat; */
  min-height: 100%;
  background-size: 100% 100%;
}
.indexPage .page2 {
  width: 100%;
  /* background: url("https://oss.sxyweb.com.cn/2021/wj/BeiGene/indexBg.png")
    center no-repeat; */
  min-height: 100%;
  background-size: 100% 100%;
}
.indexPage .header-list {
  width: 100%;
  display: flex;
}
.indexPage .header-list img {
  width: 100%;
  display:block;
  margin: 0 auto;
  margin-top: 16px;
}


.slogan-box{
  width: 100%;
  margin: 0 auto;

}
/* TODO */
/* .slogan-box div{
  width: 80.4%;
  margin: 0 auto;
  margin-top: 35px;
} */
.slogan-box div{
  width: 70.04%;
  margin: 0 auto;
  margin-top: 35px;
}
.slogan-box div img{
  width: 100%;
}
.indexPage .text-login-box {
  width: 100%;
  text-align: center;
}
.indexPage .text-login-box div{
  width: 90%;
  margin: 0 auto;
  margin-top: 60px;
}
.indexPage .text-login-box div img{
  width: 100%;
}

.select-box{
  width: 100%;
  text-align: center;
}
.select-box .select-list{
  width: 327px;
  margin: 0 auto;
  margin-top: 47px;
}
.select-box .select-list img{
  width: 100%;
}
.select-team-box{
  width: 100%;
}
.select-team-box .select-team-content{
  width: 90%;
  background: url('../assets/img/index/selectBg.png') center no-repeat;
  background-size: 100% 100%;
  margin: 0 auto;
  padding: 27px 50px;
  margin-top: 27px;
  box-sizing: border-box;
}
.select-team-box .select-team-content .list{
  width: 100%;
  display: grid;
  text-align: left;
  grid-template-columns: 440px 176px;
  margin-top: 27px;
}
.select-team-box .select-team-content  .list-item1{
  white-space:nowrap;
  overflow:hidden;
  text-overflow:ellipsis;
  font-size: 24px;
}
.select-team-box .select-team-content  .list-item2 button{
  min-width: 18vw;
  background-image: linear-gradient(to right, #ffb219 , #ff950c);
  color: #fff;
  font-size: 20px;
  font-family: Alibaba PuHuiTi, Alibaba PuHuiTi-Bold;
  font-weight: 700;
  text-align: center;
  color: #ffffff;
  letter-spacing: 0.5px;
  border: none;
  padding: 5px 25px;
  border-radius: 25px;
}
.Click-to-view-ranking-box{
  width: 100%;
}
.Click-to-view-ranking-box div{
  width: 206px;
  margin: 0 auto;
  margin-top: 27px;
}
.Click-to-view-ranking-box div img{
  width: 100%;
}
.rank-tip-box{
  width: 100%;
}
.rank-tip-box div{
  width: 415px;
  margin: 0 auto;
  margin-top: 27px;
}
.rank-tip-box div img{
  width: 100%;
}
.current-participating-teams-box{
  width: 100%;
}
.current-participating-teams-box .participating-teams-title{
  width: 499px;
  height: 59px;
  line-height: 59px;
  margin: 0 auto;
  background: url('../assets/img/index/teamBg.png') center no-repeat;
  background-size: 100% 100%;
  margin-top: 27px;
  font-family: Alibaba PuHuiTi, Alibaba PuHuiTi-Medium;
  font-weight: 500;
  text-align: justifyCenter;
  color: #777777;
  font-size: 20px;
  white-space:nowrap;
  overflow:hidden;
  text-overflow:ellipsis;
}
.current-participating-teams-box .participating-teams-title span{
  font-size: 22px;
  font-family: Alibaba PuHuiTi, Alibaba PuHuiTi-Medium;
  font-weight: 600;
  color: #009493;
}
.score-title-box{
  width: 100%;
}
.score-title-box div{
  width: 326px;
  margin: 0 auto;
  margin-top: 43px;
}
.score-title-box div img{
  width: 100%;
}
.indexPage .score-title {
  width: 100%;
  text-align: center;
  font-size: 52px;
  font-weight: 900;
  color: #9f004c;
  font-style: oblique;
}
.indexPage .score-title img {
  width: 133px;
  vertical-align: middle;
  margin: 0 8px;
}
.indexPage .tipText {
  width: 100%;
  text-align: center;
  margin-top: 80px;
}
.indexPage .tipText p {
  font-size: 52px;
  font-weight: 900;
  color: #999;
}
.indexPage .tipText img {
  width: 133px;
  vertical-align: middle;
  margin: 0 8px;
}
.indexPage .participating-team-box {
  width: 100%;
  margin-top: 30px;
}
.indexPage .participating-team {
  width: 73%;
  font-size: 28px;
  margin: 0 auto;
  background: #fff;
  border-radius: 40px;
  border: 0.1em solid #9f004c;
  font-style: oblique;
  padding: 10px 30px;
  color: #ac2264;
}
.indexPage .participating-team span {
  color: #000;
}
.indexPage .participating-team span {
  margin: 0 10px;
}
.indexPage .score-content {
  width: 83%;
  margin: 0 auto;
  border-radius: 27px;
  background: #fff;
  margin-top: 70px;
  padding: 0 33px;
  padding-bottom: 24px;
  box-sizing: border-box;
  position: relative;
  padding-top: 45px;
}
.indexPage .score-content1 {
  background: url('../assets/img/index/scrollBg.png') center no-repeat;
  background-size: 100% 100%;
}
.indexPage .score-content2 {
  background: url('../assets/img/index/scrollBg2.png') center no-repeat;
  background-size: 100% 100%;
}
.indexPage .score-content3 {
  background: url('../assets/img/index/scrollBg3.png') center no-repeat;
  background-size: 100% 100%;
}
.indexPage .score-content .title {
  width: 443px;
  margin: 0 auto;
  border-radius: 10px;
  color: #008382;
  font-weight: 800;
  font-size: 28px;
  z-index: 1000;
  position: absolute;
  
  left: 15%;
}
.indexPage .score-content1 .title{
  top: -10px;
}
.indexPage .score-content2 .title{
  top: -20px;
}
.indexPage .score-content3 .title{
  top: -20px;
}
.indexPage .score-content .title span:nth-of-type(1) {
  width: 10px;
  height: 10px;
  background: #e72876;
  display: inline-block;
  border: 0.1em solid #e0e0e0;
  border-radius: 4px;
  position: absolute;
  left: -10px;
  top: 20px;
  z-index: 100;
}
.indexPage .score-content .title span:nth-of-type(2) {
  width: 10px;
  height: 10px;
  background: #e72876;
  display: inline-block;
  border: 0.1em solid #e0e0e0;
  border-radius: 4px;
  position: absolute;
  right: -10px;
  top: 20px;
  z-index: 100;
}
.indexPage .score-content .list .list-title {
  font-size: 24px;
  color: #1f3448;
  text-align: left;
  margin-top: 30px;
}
.indexPage .slide-bar-content {
  width: 98%;
  height: 100px;
  margin: 0 auto;
  background: #c0e9e9;
  margin-top: 20px;
  border-radius: 10px;
  display: flex;
  padding: 0 8px;
  position: relative;
}
.indexPage .valueNum {
  min-width: 28px;
  box-sizing: border-box;
  height: 28px;
  position: absolute;
  left: 30px;
  top: -10px;
  background: #ce2369;
  display: inline-block;
  color: #fff;
  border-radius: 5px;
  font-size: 20px;
  line-height: 28px;
  border: 0.1em solid #fff;
}
.indexPage .slide-bar-content .small-num p:nth-of-type(1) {
  margin-top: 20px;
}
.indexPage .slide-bar-content .max-num p:nth-of-type(1) {
  margin-top: 20px;
}
.indexPage .slide-bar-content .small-num p {
  font-size: 20px;
  color: #396775;
}
.indexPage .slide-bar-content .max-num p {
  font-size: 20px;
  color: #019394;
}
.indexPage .slide-bar {
  flex: 2;
  height: 44px;
  line-height: 44px;
  border-radius: 40px;
  padding-top: 27px;
  margin: 0 14px;
  box-sizing: border-box;
}
.indexPage .slide {
  box-shadow: #beafb5 5px 5px 5px;
}
.indexPage .custom-button {
  width: 68px;
  height: 68px;
  border-radius: 100%;
  color: #fff;
  font-size: 28px;
  line-height: 68px;
  text-align: center;
  background: url("../assets/img/index/btnBg.png") center no-repeat;
  background-size: 100% 100%;
}
.indexPage .van-slider {
  width: 90%;
  box-sizing: border-box;
  margin: 0 auto;
  height: 42px;
  background-image: linear-gradient(#e72876, #c01160);
  border: 0.5em solid #ececec;
}

.indexPage .footer {
  width: 100%;
}
.indexPage .footer img {
  width: 60px;
  margin-top: 40px;
  margin-bottom: 58px;
  animation: bounce-down 1s linear infinite;
  animation: bounce-down 1s linear infinite;
}
@keyframes bounce-down {
  25% {
    transform: translateY(-28px);
    opacity: 0.1;
  }
  50% {
    transform: translateY(0);
    opacity: 0.3;
  }
  75% {
    transform: translateY(28px);
    opacity: 0.5;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
.indexPage .outLogin {
  width: 100%;
  margin-top: 204px;
}
.indexPage .outLogin button {
  width: 170px;
  height: 45px;
  margin: 0 7px;
  margin-bottom: 84px;
  font-size: 24px;
  color: #fff;
  border-radius: 20px;
  border: 0.1em solid #e8e8e8;
  box-shadow: #999 10px 10px 10px;
  background-image: linear-gradient(#e72876, #c01160);
}
.indexPage .sum-of-scores-box {
  width: 100%;
  margin-top: 36px;
  padding-bottom: 105px;
  box-sizing: border-box;

}
.indexPage .sum-of-scores-box .title {
  width: 614px;
  height: 70px;
  margin: 0 auto;
  line-height: 70px;
  background: url('../assets/img/index/teamBg.png') center no-repeat;
  background-size: 100% 100%;
  font-size: 36px;
  font-family: Alibaba PuHuiTi, Alibaba PuHuiTi-Bold;
  font-weight: 700;
  text-align: justifyCenter;
  color: #009999;
}
.indexPage .sum-of-scores-box  .content-btn{
  width: 204px;
  margin: 0 auto;
  margin-top: 28px;
}
.indexPage .sum-of-scores-box  .content-btn img{
  width: 100%;
}
.indexPage  .pingfenlishi {
 width: 100%;
 margin-top: 20px;
 padding-bottom: 80px;
 box-sizing: border-box;
}
.indexPage  .pingfenlishi img{
  width:213px
}
.indexPage .pingfenlishi button {
  width: 162px;
  height: 44px;
  border: none;
  /* background: url("https://oss.sxyweb.com.cn/2021/wj/BeiGene/pingfBtn.png")
    center no-repeat;
  background-size: 100% 100%; */
  background-image: linear-gradient(#e42775, #b40958);
  border: 0.1em solid #fff;
  box-shadow: #999 10px 10px 10px;
  border-radius: 40px;
  color: #fff;
  font-size: 24px;
  margin: 0 10px;
}
.indexPage .sum-of-scores div {
  flex: 1;
  height: 100px;
  line-height: 90px;
  padding-left: 42px;
  box-sizing: border-box;
}
.indexPage .sum-of-scores div:nth-of-type(1) {
  color: #fff;
  font-size: 30px;
  flex: 3;
  text-align: left;
}
.indexPage .sum-of-scores div:nth-of-type(2) {
  flex: 1;
  color: #e12773;
  font-size: 24px;
  line-height: 90px;
  text-align: center;
  padding-right: 30px;
  box-sizing: border-box;
}

</style>