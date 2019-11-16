<template>
  <div class="bank">
  <x-header class="headers" :left-options="{showBack: false}">
      <a slot="left" class="backBox"  @click="goBack()">
        <i class="iconfont icon-back1"></i>
        <span></span>
      </a>
    <div class="header_title">{{$route.meta.title}}</div>
    <!-- v-if="> 0" -->
    <span   slot="right" @click="topwd(data.status)">
    <!-- <span slot="right" @click="topwd()"> -->
      设置支付密码
    </span>
  </x-header>
    <div class="bannenrs">
      <img :src="bannenrs" alt="">
      <div class="balance">
        <div class="price">
          <div class="price_pic">
            <img :src="price_pic" alt="">
          </div>
          <div class="prices">{{data.credit1}}</div>
        </div>
        <div class="price_txt">
          购物通积分(CSPC)
        </div>
      </div>
    </div>
    <div class="content">
      <div class="content_top">
        <div class="logos lt">
           <img :src="use " alt="">
        </div>
        <div class="integral_list lt">
          <div class="integralnums lt">
            <div class="integral_title">可用积分数量</div>
            <div class="integralnum">{{data.ava_balance}}</div>
          </div>
          <div class="integral_box rt">
              <div class="integralbenmx tobtn rt"  @click="tomingxi">明细</div>
              <div class="integralbenzc tobtn rt" @click="jumpTo('/outintegral')">转出</div>
          </div>
        </div>
      </div>
      <div class="content_top">
        <div class="logos lt">
           <img :src="nouse " alt="">
        </div>
        <div class="integral_list lt">
          <div class="integralnums lt">
            <div class="integral_title">冻结积分数量</div>
            <div class="integralnum">{{data.freezed}}</div>
          </div>
          <div class="integral_box rt">
              <div class="integralbenmx tobtn rt">明细</div>
              <!-- <div class="integralbenmx tobtn rt" @click="tomingxi">明细</div> // 暂时关闭跳转 -->
              <!-- <div class="integralbenzc tobtn rt">转出</div> -->
          </div>
        </div>
      </div>
        <div class="tobankbox">
        <div class="toshangcheng lt">
          <div class="box">
            <div class="pics">
              <img :src="gwtpic" alt="">
            </div>
            <div class="txt" @click="jumpTo('/mall/home')">
              购物通商城
            </div>
          </div>
        </div>
        <div class="toshangcheng rt" @click="jumpTo('/bank')">
          <div class="box">
            <div class="pics">
              <img :src="yhpic" alt="">
            </div>
            <div class="txt">
              积分银行
            </div>
          </div>
        </div>
      </div>
      <div class="content_list_box">
        <div class="em">
          <div class="em_left ">钱包地址:</div>
          <div class="em_right " @click="jumpTo('/bankdetailed')">{{data.account}}</div>
        </div>
        <div class="content_list " >
          <div class="list_txt lt" @click="toContacts(data.account)">
            复制钱包地址
          </div>
          <div class="list_txt rt bl" @click="toContacts(data.privatekey)">
             复制秘钥
          </div>
        </div>
      </div>
    </div>
    <div class="tips">
      <div class="tipstitle">
        提示：
      </div>
      <!-- <ul>
        <li class="tiplist">
          1.请注意保存秘钥地址，可点击复制秘钥到记事本，记录在安全的纸质媒介上。
        </li>
        <li class="tiplist">
          2.秘钥一定不可轻易泄露，秘钥丢失被盗，积分资产可能丢失。
        </li>
      </ul> -->
    </div>
  </div>
</template>
<script>
export default {
  name:"shoppingnum",
  data(){
    return {
      bannenrs:require("../../assets/img/my/bannenrs.png"),
      price_pic:require("../../assets/img/my/price_pic.png"),
      use:require("../../assets/img/my/use.png"),
      nouse:require("../../assets/img/my/nouse.png"),
      gwtpic:require("../../assets/img/my/gwtpic.png"),
      yhpic:require("../../assets/img/my/yhpic.png"),
      data:""
    }
  },
  methods:{
    // 获取信息 amoy/chain/wallet-info
    getIngo(){
       this.$http.post('/amoy/chain/wallet-info', {
        }, false, true).then((res) => {
        if (res.code === 0) {
          this.data = res.data;
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    },
    topwd(v){
      this.$router.push({
        path:"/setuppwd",
        query:{
          state:v
        }
      })
    },
    // 复制粘贴
    toContacts(v){
      console.log(v,"复制的内容")
      if(v !=='' ){
        this.$vux.toast.text("已复制到剪贴板")
        let clipBoard = api.require('clipBoard')
        clipBoard.set({
          value: v
        }, function (ret, err) {
          if (ret) {
            console.log(ret)
          } else {
            this.$vux.toast.text("复制失败")
          }
        })
      }else{
         this.$vux.toast.text("复制的内容为空")
      }
    },
    tomingxi(){
          let browser = api.require('webBrowser')
          browser.open({
            url: this.data.url,
            titleBar: {
              textColor: '#333',
              bg: '#fff'
            }
         })
    }
  },
  mounted:function(){
    this.getIngo()
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
          font-size: .3rem;
        }
      }
    }
    .content{
      padding: 0.1rem .2rem;
      min-height: 8rem;
      .content_top{
        width: 100%;
        height: 1.1rem;
        text-align: center;
        background: #fff;
        border-radius: .15rem;
        font-size: .3rem;
        overflow: hidden;
        margin-top: .1rem;
        .logos{
          width: 1.1rem;
          height: 1.1rem;
        }
        .integral_list{
          margin-left: .2rem;
          width: 5.5rem;
          padding-top: .1rem;
          box-sizing: border-box;
          .integralnums{
            
            width: 50%;
            text-align: left;
            .integral_title{
              color: #979797;
            }
          }
          .integral_box{
            width: 50%;
            padding-top: .25rem;
            .tobtn{
              width: .9rem;
              height: .4rem;
              line-height: .4rem;
              text-align: center;
              border:1px solid #ff6354;
              color: #ff6354;
              border-radius: .5rem;
              margin-left: .2rem;
            }

          }
        }
      }
      .content_list_box{
        min-height: .9rem;
        background: white;
        border-radius: .15rem;
        margin-top: .2rem;
      
        .em{
          height: 1.25rem;
          padding: .3rem 0  0 .2rem;
          box-sizing: border-box;
          .em_left{
            font-size: .3rem;
            color: black;
          }
          .em_right{
            color: #979797; 
            font-size: .25rem;

          }
        }
        .content_list{
          height: 1rem;
          box-sizing: border-box;
          border-top: 1px solid #eeeeee;
          .list_txt{
            width: 50%;
            box-sizing: border-box;
            color: #ff6958;
            font-size: .2rem;
            text-align: center;
            height: 1rem;
            line-height: 1rem;
          }
          .bl{
            border-left: 1px solid #eeeeee;
          }
        }
      }
      .tobankbox{
        margin-top: .2rem;
        height: 1.1rem;
        
        .toshangcheng{
          width: 3.45rem;
          height: 1.1rem;
          background: white;
          border-radius: .15rem;
          padding: .3rem 0 0 .7rem ;
          box-sizing: border-box;
           .box{
              display: flex;
              flex-wrap: wrap;
              margin: 0 auto;
            .pics{
              width: .4rem;
              height: .35rem;
            }
            .txt{
              margin-left: .1rem;
            }
          }
        }
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
      .vux-header-right span{
      color:#000;
    }
</style>


