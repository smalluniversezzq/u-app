<template>
  <div class="operatorcenter" >
    <x-header class="headers" :left-options="{showBack: false}">
        <a slot="left" class="backBox"  @click="goBack()">
          <i class="iconfont icon-back1"></i>
          <span>返回</span>
        </a>
      <div v-if="tabstate ==='operatorcenter'" class="header_title">{{$route.meta.title}}</div>
      <div v-if="tabstate ==='hehuoren'" class="header_title">{{'合伙人中心'}}</div>
      <span slot="right">
      </span>
    </x-header>
    <div class='operator_info'>
      <div class="info" >
        <div class="user_info_box">
          <div class="user_info">
            <div class="logo">
              <img  :src="data.avatar" alt="">
            </div>
            <ul class='infos'>
              <li class="username">
                {{data.realname}}
              </li>
              <li class="userid">
                店长级别：{{data.lv}}
              </li>
              <li class="userid">
                店长ID号：{{data.nickname}}
              </li>
            </ul>
          </div>
          <div class="balance">
            <p class="balancetxt rt">账户余额</p>
            <p class="money rt">{{data.credit3}}</p>
            <div class="money_btn rt" @click="jumpTo('/profitDetail')">查看详情</div>
          </div>
        </div>
        <div class="bill">
          <div class="obtain">
            <p class="obtain_title">30日内已到账收入</p>
            <p class="money">{{data.lately_month_settlement}}</p>
            <p class="tolist"  @click="jumpTo('/ordercenter')">点击查看列表</p>
          </div>
            <div class="notget">
            <p class="obtain_title">30日内未到账收入</p>
            <p class="money">{{data.lately_month_unsettlement}}</p>
            <p class="tolist" @click="jumpTo('/ordercenter')">点击查看列表</p>
          </div>
        </div>
      </div>
      <div class="time_box">
        <div class="statistics">
          <p @click="jumpTo('/profit')">收益统计</p>
          <div class="helppic"  @click="openalert()">
            <img  :src="helppic" alt="">
          </div>
        </div>
        <div class="settime">
          <Group>
            <Datetime v-model ="startValue" :clear-text='startTxt'  :end-date="endValue"> </Datetime>
            <!-- <span class='vux-cell-value'>—</span> -->
            <span>&nbsp;&nbsp;</span>
            <Datetime v-model ="endValue" :clear-text='endTxt' :start-date="startValue"></Datetime>   
            <!-- <datetime v-model="limitHourValue" :start-date="startDate" :end-date="endDate" format="YYYY-MM-DD" @on-change="change" :title="test"></datetime> -->
          </Group>
          <div class="timebtn rt" @click='time()'>查询</div> 
          <!-- <div class="time" v-model ="value">-2018/11/07</div> -->
        </div>
      </div>
    </div>
    <div class="subject">
      <div class="commission">
        <div class="commissions">
          <div class="comtitle">预估佣金</div>
          <div class="commoney">{{data.thismonth_payment}}</div>
        </div>
        <div class="commnum">
          <div class="comtitle">订单数量</div>
          <div class="commoney">{{data.thismonth_order}}</div>
        </div>
      </div>
        <dealList :state="tb" :data='tbdata' ></dealList >
        <dealList :state="pd" :data='pdddata'  ></dealList >
        <dealList :state="jd"  :data='jddata'  ></dealList >
      </div>
  </div>
</template>
<script>
import {Group, Datetime ,DatetimeRange } from 'vux';
import dealList from '../../components/dealList'
import * as utils from '../../utils';
import { debug } from 'util';
export default {
  name:"operatorcenter",
  components:{
    Group,
    Datetime,
    DatetimeRange ,
    dealList
  },
  data(){
    return {
      pdddata:"",
      jddata:"",
      tbdata:"",
      tb:"tb",
      jd:"jd",
      pd:"pd",
      nowDate:"",
      createvalue:"",
      endvalue:"",
      createTimes:false,
      value:"",
      test:"ceshi",
      limitHourValue:"",
      startValue:'2019-01-01',
      endValue:'2019-12-31',
      startTxt:"开始时间",
      endTxt:"结束时间",
      helppic:require("../../assets/img/my/help.png"),
      data:"",
      startTime:"",
      endTime:"",
      tabstate:"",
    }
  },
  created(){
  },
  mounted(){
    this.getcenter();
    this.gettime();
    this.tabstate = this.$route.query.state;
    console.log(this.tabstate)
  },
  methods:{
      timer(){
        setInterval(() => {
          console.log(this.createvalue)
        },100)
      },
      //amoy/shop/get-orderinfo
      time(){
        this.$http.post('/amoy/shop/get-orderinfo', {
          start_time:this.startValue,
          end_time:this.endValue
          }, false, true).then((res) => {
          if (res.code === 0) {
            this.tbdata = res.data.tb_info;
            this.pdddata = res.data.pdd_info;
            this.jddata = res.data.jd_info;
          }else{
            this.$vux.toast.text(res.msg)
          }
        })
      },
    openalert(){
        this.$store.commit('setshowoperator',true);
        window.canRightSlipBack = false
    },
    setcreateTime(val){
       let _this = this;
       _this.timer()
      _this.$vux.datetime.show({
        confirmText:"确认",
        clearText:"选择开始时间",
        format: 'YYYY-MM-DD',
        cancelText:"取消选择",
        value:"2017-05-20 18", // 其他参数同 props
        onHide(val){
        },
        onShow () {
        },
        onConfirm (val){
          console.log(val)
        },
        onChange(){
        },
      })
    },
    // 获取信息amoy/shop/personal-center、
    getcenter(){
      this.$http.post('/amoy/shop/personal-center', {
        }, false, true).then((res) => {
        if (res.code === 0) {
          this.data = res.data;
          this.tbdata = res.data.tb_info;
          this.pdddata = res.data.pdd_info;
          this.jddata = res.data.jd_info;
          console.log(this.data,"______________")
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    },
    tolist(){

    },

    // setendTime(){
    //     // alert("执行了")
    //     _this.$vux.datetime.show({
    //     value: _this.endvalue, // 其他参数同 props
    //     title:"",
    //     confirmText:"确认",
    //     clearText:"选择结束时间",
    //     cancelText:"取消选择",
    //     onHide () {
    //       // const _this = this
    //     },
    //     onShow () {
    //       // const _this = this
    //     },
    //     onConfirm (){
    //       let _this = this;
    //       console.log(_this.endvalue);
    //        _this.createTimes = false;
    //     }
    //   })
    // },
    // goBack(){

    // }
    gettime(){
      var date = new Date();
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var day = date.getDate();
      if (month < 10) {
          month = "0" + month;
      }
      if (day < 10) {
          day = "0" + day;
      }
      this.nowDate = year + "-" + month + "-" + day;
      console.log(this.nowDate,typeof(this.nowDate))
    }
  }
}
</script>
<style scoped lang='less'>
*{margin: 0;padding: 0;}
  .operatorcenter{
    width: 100%;
    min-height: 100%;
    background:#eeefef;
    .vux-header{
    position: relative !important;
    background: #ff5858 !important;
    color: #fff !important;
    border: none !important;
      .vux-header-left{
        .vux-header-back{
        }
        .backBox{
          color: #fff;
          font-size: .3rem;
          i{
          color: #fff !important;
          font-size: .3rem !important; 
          }
        }
      }
      // .vux-header-title{
      //   text-align:left !important;
      //   color:#fff;
      // }
      .vux-header-right{
        i{
          display:inline-block; 
          width:.08rem;
          height:.08rem;
          background-color:#fff;
          margin:0.02rem ;
          border-radius:50%;
        }
      }
    }
    .operator_info{
      width:100%;
      min-height:5rem;
      background: #fff;
      .info{
        min-height: 4.5rem;
        // background: #02a890;
         background: linear-gradient(to right,#ff3f61,#ff7f60);
        padding:.3rem .2rem;
        box-sizing: border-box;
        .user_info_box{
          display: flex;
          flex-wrap: wrap;
          justify-content: space-between;
          .user_info{
            width: 4.2rem;
            height: 1.2rem;
            padding: 0.16rem;
            display: flex;
            flex-wrap: wrap;
            box-sizing: border-box;
            .logo{
              width: 1.1rem;
              height: 1.1rem;
              border-radius: 50%;
              background:#fff;
              overflow: hidden;
              img{
                width: 100%;
              }
            }
            .infos{
              width: 2.5rem;
              margin-left:.2rem;
              color: #fff;
              .username{
                font-size: .3rem;
              }
            }
          }
          .balance{
            width: 2.68rem;
            height: 1.5rem;
            background: #f7b52c;
            border-radius: .06rem;
            padding: .1rem .2rem;
            box-sizing: border-box;
            color: #fff;
            .balancetxt{
              font-size:.2rem;
              width: 100%;
              text-align: right;
            }
            .money{
              font-size:.45rem;
              height:.5rem;
              line-height:.5rem;
              width: 100%;
              text-align: right;
            }
            .money_btn{
              width: 1.15rem;
              height: .4rem;
              background: #e95513;
              font-size: .2rem;
              line-height: .4rem;
              text-align: center;
              border-radius: .05rem;
              margin-top: .02rem;
            }
          }
        }
        .bill{
          height: 2rem;
          background: #fff;
          border-radius: .05rem;
          margin-top: .35rem;
          padding: .25rem;
          display: flex;
          flex-wrap: wrap;
          box-sizing:border-box;
          .obtain{
            width: 50%;
            height:1.40rem;
            border-right:1px solid #e5e6e6 ;
            box-sizing: border-box;
            .obtain_title{
              font-size: .25rem;
              color: #ea5514 ;
            }
            .money{
              font-size: .4rem;
            }
            .tolist{
              font-size: .25rem;
              color: #99999a;
            }
            p{
              text-align: center;
            }
          }
         .notget{
            width: 50%;
            height:1.40rem;
            .obtain_title{
              font-size: .25rem;
              color: #c21a22;
            }
            .money{
              font-size: .4rem;
            }
            .tolist{
              font-size: .25rem;
              color: #99999a;
            }
            p{
              text-align: center;
            }
          }
        }
      }
      .time_box{
        height: 0.7rem;
        background: #fff;
        display: flex;
        flex-wrap: wrap;
        padding: 0.17rem .2rem;
        box-sizing:border-box;
        .statistics{
          width: 2.3rem;
          height: .4rem;
          display: flex;
          flex-wrap: wrap;
          p{
            color: #fff;
            width: 1.3rem;
            height: .4rem; 
            border-radius: .05rem ;
            background: #8fc31f;
            // line-height: .4rem;
            text-align: center;
          }
          .helppic{
            width: 0.3rem;
            height: 0.3rem;
            line-height: .3rem;
            margin: .05rem 0 0 .1rem;
          }
        }
        .settime{
          color:#fff;
          width:4.5rem;
          height:.4rem;
          // border-radius: .05rem;
          display: flex;
          flex-wrap: wrap;
          justify-content: space-between;
          background:#595757;
          border-radius: .05rem;
          padding-left: .1rem;
          overflow: hidden;

          .time{
            width: 75%;
            height: .4rem; 
            background: #595757;
            border-radius: .05rem ;
            font-size: .2rem;
            text-align: center;
          }
          .timebtn{
            width:25%;
            height: .4rem; 
            background:#f39800; 
            text-align: center;
            border-radius: .05rem ;
          }
        }
      }
    }
    .subject{
      padding: .2rem;
      .commission{
        height: 1.2rem;
        background: #fff;
        border-radius: .05rem ;
        display: flex;
        flex-wrap: wrap;
        padding: .1rem;
        box-sizing: border-box;
        margin-bottom: .2rem;
        .commissions{
          height: 1rem;
          width: 50%;
          border-right:1px solid #e5e6e6 ;
          box-sizing: border-box;
          text-align: center;
          .comtitle{
            color:#e73828; 
          }
          .commoney{
            font-size: .35rem;
            font-weight: 800;
          }
        }
        .commnum{
          height: 1rem;
          width: 50%;
          box-sizing: border-box;
          text-align: center;
          .comtitle{
            color:#e73828; 
          }
          .commoney{
            font-size: .35rem;
            font-weight: 800;
          }
        }
      }
    .tb{
      height:3.4rem;
      background: #fff;
      padding: .1rem;
      box-sizing: border-box;
      margin-bottom: .2rem;
      }
      .em{
        height: .8rem;
        border-radius:.05rem;
        line-height: .8rem;
        padding: 0 .2rem;
        background: #e73828;
        font-size: .22rem;
        color: #fff;
      }
      .commissionlist_box{
        display: flex;
        flex-wrap: wrap;
        margin-top: .25rem;
        .commission_list{
          width: 33%;
          border-right: 1px solid #e5e6e6;
          padding-left: .3rem;
          box-sizing: border-box;
          height: 1rem;
          p{
            color: #666666;
          }
          .money{
            font-size: .3rem;
            font-weight: 800;
            text-align: left;
          }
        }
      }
    }
  }
  .header_title{
    color:#fff;
    text-align: left;
  }
  .lt{float:left;}
  .rt{float:right;}
</style>
<style  lang='less'>
// weui-cell__ft vux-cell-primary vux-datetime-value
// vux-cell-value

.settime{
  div{
  .weui-cells{
    margin-top:0 !important;
    background-color: #595757 !important; 
    line-height: 1.41176471; 
    font-size: .2rem !important;
    overflow: hidden;
    position: relative;
    display: flex;
    flex-wrap: wrap;
    .weui-cells:before,.weui-cells:after{
      border: none !important;
    }
    // 同级
    .vux-datetime{
      width: 1.5rem;
      height: .4rem;
      background: #595757;
      padding: 0 !important;
      .weui-cell__ft{
      text-align: center !important;
      padding-right: 0 !important;
      .vux-cell-value{
          color: #fff  !important;
        }
      }
    }
    .weui-cell_access{
    }
  }
}

 

  .weui-cell1{
     padding: 0 !important;
    position: relative;
    display: flex;
    align-items: center;
  }
  .vux-datetime-value{
    background: #595757!important;
  }
  .weui-cell_access:active{
     background: #595757!important;
  }
  .weui-cell__ft:after,.weui-cell:before{
    display: none !important;
  }
  .weui-cell__ft{
    
  }
  .vux-datetime-clear,.vux-datetime-confirm{
    color: #828282!important;
  }
  .times{
    height: .3rem;
  }
}
</style>



