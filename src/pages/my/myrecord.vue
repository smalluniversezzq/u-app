<template>
  <div class="bankdetailed">
    <div class="containers">
      <ul class="bank_list_box">
        <li class="lists em" v-for="(item,index) in recordList" :key="index">
          <div class="list_txt lt">
            <p class="list_title">{{item.remark}}</p>
            <!-- <div class="ddh lt">订单号：13216548465414 </div><div class="copi lt">复制</div> -->
            <div class=" corend">{{item.created_at}}</div>
          </div>
          <div class="times rt">
            <div class="yifafang">已发放</div>
            <!-- <div class="xiangqing" @click="showalert()">详情</div> -->
          </div>
        </li>
      </ul>
    </div>
    <div class="bosh">没有更多了~</div>
    <!-- :values='valueinfo' -->
    <Alert  ></Alert>
  </div>
</template>
<script>
import Alert from '../../components/Alert'
export default {
  name:"myrecord",
  components:{
    Alert
  },
  data(){
    return {
      shows:false,
      valueinfo:"",
      recordList:"",
        //       {
        //     "price":"9.00",
        //     "remark":"每日抽奖获得积分9,已存入积分银行",
        //     "created_at":"2019-05-06 10:48:08",
        //     "nickname":"6IOh54yb",
        //     "realname":null
        // },
    }
  },
  methods:{
    showalert(){
       this.$store.commit('setshowrules',true)
    },
  //获取个人中奖名单amoy/lucky/my-record 
    getMyRecord(){
      this.$http.post('/amoy/lucky/my-record', {
      }, false, true).then((res) => {
        if (res.code === 0) {
          this.recordList = res.data
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    },
  },
  mounted(){
    this.getMyRecord()
  }
}
</script>
<style scoped lang='less'>
  .bankdetailed{
   .containers{
     margin-top: .2rem;
     background: #fff;
     .bank_list_box{
       .em{
          min-height: 1.1rem;
          padding: 0 .2rem;
          border-bottom: 1px solid #eeeeee;
          padding: .3rem;
          .list_txt{
            min-height: 1.1rem;
            .list_title{
              font-size: .3rem;
            }
            .ddh{
              margin-right: .1rem;
         
            }
            .copi{
                width: .8rem;
                height: .4rem;
                text-align: center;
                line-height: .4rem;
                color: #ff5845;
                background: #ffeeec;
                border-radius: .2rem;

              }
            .corend{
              font-size: .2rem;
              color: #b0b0b0;
            }
          }
          .times{
            .yifafang{
              text-align: center;
            }
            .xiangqing{
              width: .8rem;
              height: .4rem;
              text-align: center;
              line-height: .4rem;
              color: #ff5845;
              border-radius: 0.1rem;
              border: 1px solid #fe5848;
              margin-top: .1rem;
            }
          }
        }
  
     }
   } 
   .bosh{
     text-align: center;
     font-size: .25rem;
     color: #b0b0b0;
     margin-top: .3rem;
   }
  }
   .lt{float: left;}
  .rt{float: right;}
  ul :nth-last-child(li){
      border:none;
  }
// {
// background:#ff0000;
// }
</style>


