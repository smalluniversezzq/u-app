<template>
  <div class="payMent">
    <div class="header">
      <x-header
        slot="header"
        :left-options="{showBack: false}"
      >
        <a @click="goBack()" slot="left" class="backBox"><i class="iconfont icon-back"></i></a>
        支付
      </x-header>
    </div>
    <div class="content">
      <div class="payMent_parse">
        <div class="left">
          订单金额
        </div>
        <div class="right">
          <span>积分</span>{{data.order ? data.order.credit : 0}}
          <span v-if='data.order.commission != 0'>佣金 {{data.order ? data.order.commission : 0}}</span>
        </div>
      </div>
      <div class="payMent_list">
        <div class="list_title">
          选择支付方式
        </div>
        <!-- <div class="list" v-for="(item,index) in payType" @click="handChange(index)" v-if="item.status" :key="index">
          <div class="img_box">
            <img v-if="index == 'credit'" src="../../../assets/mall/img/home/yue.png" alt="">
            <img v-if="index == 'alipay'" src="../../../assets/mall/img/home/apliy.png" alt="">
            <img v-if="index == 'wechat'" src="../../../assets/mall/img/home/wheat.png" alt="">
            <span>{{item.title}}</span>
          </div>
          <div class="checkbox">
            <img :src="active == index ? './static/libs/img/active.png' : './static/libs/img/active_no.png'" alt="">
          </div>
        </div> -->
        <div class="list" @click="handChange(1)" >
          <div class="img_box">
            <img  src="../../../assets/mall/img/home/yue.png" alt="">
            <span>积分支付</span>
          </div>
          <div class="checkbox">
            <img :src="active == 1 ? './static/libs/img/active.png' : './static/libs/img/active_no.png'" alt="">
          </div>
        </div>
      </div>
      <div class="btns">
        <div class="btn" @click="handBuy">立即购买</div>
      </div>
    </div>
    <div class="prizealert"  v-show="prizealert" >
      <div class="alert">
        <div class="prizetitle">请输入密码</div>
        <div class="prizecontent">
          <div class="int_box">
            <div class="int_txt lt">输入密码</div>
            <div class="inpu lt">
              <input type="password" class="int  lt" v-model="pay_password" placeholder="请输入密码">
            </div>
          </div>
           <div class="contents" @click="setpay">支付</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as apiHttp from '../../../api/index'
import * as utils from '../../../utils'
import axios from 'axios'
export default {
  name: 'payMent',
  data () {
    return {
      active: '1',
      data: {},
      payType: {},
      prizealert:false,
      pay_password:"",
    }
  },
  mounted: function () {
    // 删除
    apiHttp.paymentDisplay(this.$route.query.orderId).then(response => {
      if (response.code === 0) {
        this.data = response.data
        this.payType = response.data.payType
      }
    })
  },
  methods: {
    handChange (key) {
      this.active = key
    },
    close(){
      this.prizealert=false
    },
    // 点击支付
    getWay (type) {
      this.$http.post('/amoy/user/upgrade-agent', {
        id: this.index,
        upgrade_type: 1,
        type: type
      }, true).then(res => {
        this.dialog = false
        if (res.code === 0) {
          if (type === 'alipay') {
            this.plusReady(1, res.data.payOrder)
          } else {
            this.plusReady(0, res.data.payOrder)
          }
        }
      })
    },
    // 获取支付通道
    plusReady (index, data) {
      let that = this
      if (index === 1) {
        let aliPayPlus
        if (this.$aliPay === 1) {
          aliPayPlus = api.require('aliPayTradePlus')
        } else {
          aliPayPlus = api.require('aliPayPlus')
        }
        aliPayPlus.payOrder({
          orderInfo: data
        }, function (ret, err) {
          console.log(JSON.stringify(err))
          switch (ret.code) {
            case '9000':
              that.$vux.toast.text('支付成功')
              that.$router.push({
                name: 'mallPaySuccess'
              })
              break
            case '6001':
              that.$vux.toast.text('取消支付')
              break
            case '8000':
              that.$vux.toast.text('正在处理中')
              break
            default:
              that.$vux.toast.text('支付失败')
          }
        })
      }else if (index === 2) {
        console.log(JSON.stringify(data))
        let wxPay = api.require('wxPay')
        let info = {
          apiKey: data.appid,
          orderId: data.prepayid,
          mchId: data.partnerid,
          nonceStr: data.noncestr,
          timeStamp: data.timestamp,
          sign: data.sign
        }
        wxPay.payOrder(info, function (ret, err) {
          if (ret.status) {
            that.$vux.toast.text('支付成功')
            that.$router.push({
              name: 'mallPaySuccess'
            })
          } else {
            console.log(JSON.stringify(err))
            if (err.code === -2) {
              that.$vux.toast.text('取消支付')
            } else {
              that.$vux.toast.text('支付失败')
            }
          }
        })
      }
    },
    // 支付
    setpay(){
      let _this = this
        if(_this.pay_password.length <= 0){
            _this.$vux.toast.text('支付密码不能为空')
         }else{
          this.$http.post('/mall/payment/pay', {
              orderId:_this.data.order.orderId,
              pay_password:_this.pay_password
        }, false, true).then(res => {
              if (res.code == 0) {
                _this.prizealert = false
                _this.$vux.toast.text(res.msg)
                utils.storage.set('orderId','2');
                _this.$router.push({name:'mallFixing_order',path:'/mall/fixing_order'});
              }else if(res.code == 1){
                 _this.prizealert = false
               _this.$vux.toast.text(res.msg)
              }
            })
         }
    },

    handBuy () {
       if(this.active == "1"){
         this.prizealert = true
       }
      // if (this.active === 'credit') {
      //   apiHttp.paymentPayType(this.$route.query.orderId, this.active).then(response => {
      //     if (response.code === 0) {
      //       // axios.post(response.data.notify)
      //       this.$router.push({
      //         name: 'mallPaySuccess'
      //       })
      //     }
      //   })
      // } else if (this.active === 'alipay') {
      //   apiHttp.paymentPayType(this.$route.query.orderId, this.active).then(response => {
      //     if (response.code === 0) {
      //       this.plusReady(1, response.data.payOrder)
      //     }
      //   })
      // } else {
      //   apiHttp.paymentPayType(this.$route.query.orderId, this.active).then(response => {
      //     if (response.code === 0) {
      //       // this.$vux.toast.text('暂未开放该支付通道')
      //       this.plusReady(2, response.data.payOrder)
      //     }
      //   })
      // }
    }
  }
}
</script>

<style scoped lang='less'>
.prizealert{
    width: 100%;
    height: 100%;
    background: rgba(76, 76, 76, .5);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 3000;
    .alert{
      width: 6rem;
      height: 3rem;
      background: #fff;
      border-radius: .2rem;
      position: absolute;
      top: 60%;
      left: 50%;
      margin-left: -3rem;
      margin-top: -4rem;
      padding: .2rem;
      box-sizing: border-box;
      .prizetitle{
        margin-top: .3rem;
        width: 100%;
        text-align: center;
      }
      .prizecontent{
        margin-top: .1rem;
        text-align: center;
        font-size: .4rem;
        color: #da2a03;
        height: 3rem;
        width: 100%;
        .int_box{
           height: 1rem;
          line-height: 1rem;
          .int_txt{
            width: 1rem;
            font-size: .25rem;
            color: #000;
            margin-right: .1rem;
          }
          .inpu{
            height: 1rem;
            line-height: .9rem;
              input{
              background: #fff;
              font-size: 0.26rem;
              outline: none;
              border: none;
              line-height: 1rem;
            }
          }
        }
        .contents{
          margin: 0 auto;
          height: .5rem;
          width: 1rem;
          line-height: .5rem;
          text-align: center;
          border-radius: 5px;
          background: #fc3357;
          color: #fff;

        }
        .contents{
          text-align: center;
          font-size: .25rem;
        }
      }
    }
}
.lt{float:left}
</style>
