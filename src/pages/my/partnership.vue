<template>
    <div class="partnership">
      <div class="partnershipBox">
        <!-- v-if="item.status ==='1' && item.upInfo.model.is_show === '1'" -->
        <div class="box" v-for="(item, index) in data" :key = index >
          <div class="top">
            <!-- <img :src="item.thumb" alt=""> -->
            <p>{{item.title}} </p>
            <span @click="upgrade(index+1)" >立即升级</span>
          </div>
          <ul class="con">
            <li class="conlist" v-for="(i,v) in item.condition"  :key="v">
                {{i}}
            </li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
import { Radio, Group } from 'vux'
export default {
  name: 'partnership',
  data () {
    return {
      data: '',
      type: '',
      dialog: false,
      index: ''
    }
  },
  components: {
    Radio,
    Group
  },
  methods: {
    getlist (inx) {
      //amoy/user/upgrade-operator   运营商信息
      this.$http.post('/amoy/user/get-conditions', {
      
      }, true).then((res) => {
        if (res.code === 0) {
          this.data = res.data
        }
      }) 
    },
    upgrade (inx) {
      //amoy/user/upgrade-operator   运营商提交
      this.$http.post('/amoy/user/upgrade-operator', {
        type:inx
      }, true).then((res) => {
        if (res.code === 0) {
            this.$router.push('/my')
        }else{
        }
      })
    },

    //支付
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
          switch (ret.code) {
            case '9000':
              that.$vux.toast.text('支付成功')
              setTimeout(() => {
                that.$router.go(-1)
              }, 2000)
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
      } else {
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
    pay (id, data) {
      plus.payment.request(this.channel, data, (result) => {
        this.$vux.toast.show('支付成功')
      }, (error) => {
        if (error.message && (error.message.indexOf('canceled') > -1 || error.message.indexOf('取消') > -1)) {
          this.$vux.toast.text('支付已取消')
        } else {
          this.$vux.toast.text('支付失败：' + JSON.stringify(error))
        }
      })
    }

  },
  mounted () {
    // 删除
    this.getlist()
  }
}
</script>

<style scoped lang="less">
  .partnership{
    .partnershipBox{
      background-image: url('../../assets/img/my/bj.png');
      -webkit-background-size: 100%;
      background-size: 100%;
      padding-top: 3.7rem;
      box-sizing: border-box;
      padding-bottom: 0.1rem;
      min-height: 100%;
    }
    position: relative;
    height: 100%;
    overflow-y: auto;
    .box{
      border-radius: 5px;
      margin: 0 .2rem;
      background-color: #fff;
      overflow: hidden;
      margin-bottom: 0.5rem;
      .top{
        padding: 0 .2rem;
        height: 1rem;
        background: #3a3a3a;
        box-sizing: border-box;
        font-size: 0.3rem;
        display: flex;
        align-items: center;
        img{
          width: .5rem;
          height: .5rem;
          margin-right: .2rem;
        }
        p{
          flex: 1;
          font-size: 0.3rem;
          color: #fff;
        }
        span{
          font-size: 0.3rem;
          color: #727272;
        }
      }
      .con{
        font-size: 0.24rem;
        color: #666;
        padding:0.2rem;
        box-sizing: border-box;
        p{
          font-size: 0.28rem;
          color: #fc3357;
        }
        .condition,.explain{
          margin: 0.2rem 0;
          img{
            max-width: 100%;
          }
        }
      }
    }
    .bot{
      position: absolute;
      width: 6rem;
      background-color: #fff;
      /*height: 2rem;*/
      left: 50%;
      transform: translateX(-50%);
      border-radius: 10px;
      top: 1rem;
      padding-bottom: 0.2rem;
      span{
        display: block;
        font-size: 0.24rem;
        width: 80%;
        padding: 0.2rem;
        box-sizing: border-box;
        margin: 0 auto;
        background-color: red;
        text-align: center;
        margin-top: 0.2rem;
        border-radius: 10px;
        font-size: 0.28rem;
        color: #fff;
      }
    }
  }
  .boxs{
    .dialog{
      background: rgba(0, 0, 0, 0.4);
      width: 100%;
      height: 100%;
      position: fixed;
      left: 0;
      top: 0;
    }
    .way{
      background: #fff;
      border-radius: .1rem;
      position: fixed;
      left: .3rem;
      top: 50%;
      margin-top: -1.7rem;
      width: 6.9rem;
      font-size: .3rem;
      .icon-close{
        position: absolute;
        right: .3rem;
        top: .25rem;
        font-size: .4rem;
      }
      p{
        text-align: center;
        height: 1.15rem;
        line-height: 1.15rem;
        justify-content: center;
        display: flex;
        align-items: center;
        position: relative;
        img{
          width: .4rem;
          margin-right: .1rem;
        }
      }
      .t:after{
        content: '';
        width: 100%;
        height: 1px;
        position: absolute;
        bottom: 0;
        right: 0;
        border-bottom: 1px solid #dadada;
        transform-origin: 0 0;
        transform: scaleY(.5);
      }
      .ways:after{
        content: '';
        width: 90%;
        height: 1px;
        position: absolute;
        bottom: 0;
        left: 5%;
        border-bottom: 1px solid #dadada;
        transform-origin: 0 0;
        transform: scaleY(.5);
      }
    }
  }
</style>
