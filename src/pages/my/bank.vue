<template>
  <div class="bank">
    <div class="bannenrs">
      <img :src="bannenrs" alt="">
      <div class="balance">
        <div class="price">
          <div class="price_pic">
            <img :src="price_pic" alt="">
          </div>
          <div class="prices">{{data.credit5}}</div>
        </div>
        <div class="price_txt">
          积分余额
        </div>
      </div>
    </div>
    <div class="content">
      <div class="content_top">
          当前积分兑换购物通倍数<span>{{data.multiple}}</span>倍
      </div>
      <div class="content_list_box">
        <div class="em">
          <div class="em_left lt">最近积分明细</div>
          <div class="em_right rt" @click="jumpTo('/bankdetailed')">详情 &gt;</div>
        </div>
        <div class="content_list  em" v-for="(item,index) in data.list" :key="index">
          <div class="list_txt lt">
            <p class="list_name lt">{{item.remark}}</p>
            <p class="lt corend">+{{item.price}}积分</p>
          </div>
          <div class="times rt">
             {{item.created_at}}
          </div>
        </div>
      </div>
      <div class="tocore" @click="jumpTo('/signIn')">
        前往任务中心
      </div>
    </div>
     <div class="tips">
        <div class="tipstitle">
          提示：
        </div>
        <ul>
          <li class="tiplist">
            1.请注意保存秘钥地址，可点击复制秘钥到记事本，记录在安全的纸质媒介上。
          </li>
          <li class="tiplist">
            2.秘钥一定不可轻易泄露，秘钥丢失被盗，积分资产可能丢失。
          </li>
        </ul>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return {
      bannenrs:require("../../assets/img/my/bannenrs.png"),
      price_pic:require("../../assets/img/my/price_pic.png"),
      data:"",
    }
  },
  methods:{
    //获取bankinfo
    getbankInfo(){
      this.$http.post('amoy/bank/index', {
        }, false, true).then((res) => {
          if (res.code === 0) {
            this.data = res.data;
            console.log(this.data)
          }else{
            this.$vux.toast.text(res.msg)
          }
        })
    }
  },
  create:function(){

  },
  mounted:function(){
    this. getbankInfo()
  }
}
</script>
<style scoped lang='less'>
  .bank{
    .bannenrs{
      height: 2rem;
      position: relative;
      .balance{
        position: absolute;
        width: 100%;
        height:1.3rem;
        top: .3rem;
        left: 0;
        text-align: center;
        .price{
          display: flex;
          flex-wrap: wrap;
          justify-content: center;
          height: .45rem;
          line-height: .45rem;
          .price_pic{
            width: .4rem;
            height: .4rem;
            margin-right: .2rem;
            img{
            vertical-align: middle;
            }
          }
          .prices{
            font-size: .45rem;
            color:white;
          }
        }
        .price_txt{
          margin-top: .3rem;
          text-align: center;
          color: white;
        }
      }
    }
    .content{
      padding: 0.1rem .2rem;
      min-height: 8rem;
      .content_top{
        width: 100%;
        height: 1rem;
        text-align: center;
        line-height: 1rem;
        background: #fff;
        border-radius: .15rem;
        font-size: .3rem;
        span{
          font-size: .4rem;
          color:#ff6759;
        }
      }
      .content_list_box{
        min-height: .9rem;
        background: white;
        border-radius: .15rem;
        margin-top: .2rem;
      
        .em{
          height: .9rem;
          line-height: .9rem;
          padding: 0 .2rem;
          .em_left{
            font-size: .3rem;
            color: black;
          }
          .em_right{
            color: #ff6958; 
            font-size: .25rem;

          }
        }
        .content_list{
          border-top: 1px solid #eeeeee;
          .list_txt{
            width: 4rem;
            height: .9rem;
            overflow: hidden;
            .list_name{
              width: 2.5rem;
            }
            .corend{
              color: #ff6958;
            }
          }
          .times{
            width: 2.7rem;
            color: #999999;
            font-size: .2rem;
            height: .9rem;
            text-align: right;
          }
        }
      }
      .tocore{
        width: 6.25rem;
        height: .9rem;
        background:  linear-gradient(to right,#f97b65, #f63f37);
        margin: .5rem auto .5rem;
        text-align: center;
        line-height: .9rem; 
        font-size: .3rem;
        color: white;
        border-radius: .5rem;
      }

    }
    .tips{
      color:#666666;
      font-size: .25rem;
      padding: 0 .2rem .5rem;
    }
  }
  .lt{float: left;}
  .rt{float: right;}
</style>


