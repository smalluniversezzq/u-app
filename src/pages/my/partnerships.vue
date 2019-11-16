<template>
    <div class="partnership">
      <!-- kkkkkkkkkkk -->
      <div class="partnershipBox">
        <div class="box" >
          <div class="top">
            <p>升级为合伙人 </p>
          </div>
          <div class="con">
            <div class="condition">
             <div class="inttit">
                姓名
             </div>
              <div class="int">
                 <input class="ints" type="text" placeholder="请输入姓名" v-model="username">
              </div>
            </div>
            <div class="condition">
             <div class="inttit">
                手机号
             </div>
              <div class="int">
                 <input  class="ints" type="text" placeholder="请输入手机号" v-model="mobile">
              </div>
            </div>
              <div class="condition">
             <div class="inttit">
                微信号
             </div>
              <div class="int">
                 <input  class="ints" type="text" placeholder="选填" v-model="wxcode">
              </div>
            </div>
            <div class="condition">
             <div class="inttit">
                选择性别 
             </div>
              <div class="int">
                 <!-- <input class="ints"  type='text' placeholder="请选择性别" v-model='sex'> -->
                <div class='issex'>
                  <span>男</span><div><input id="intrad" value="0" name="sex" style="appearance: normal"  type="radio"  @change="changes"  checked="checked"  v-model='sex' :style="{background:sex==='0'? colors:'#949494'}" /></div>
                  <span>女</span><div><input id="intrad"  value="1" name="sex" style="appearance: normal" type="radio" @change="changes" v-model='sex' :style="{background:sex==='1'? colors:'#949494'}"/></div>
                </div>
              </div>
            </div>
            <div class="condition">
             <div class="inttit">
                选择地区
             </div>
              <div class="int">
                 <input  class="ints" type="text" placeholder="请输入地区" v-model="region">
              </div>
            </div>
           <!-- <div class="condition">
             <div class="inttit">
                合伙人邀请码
             </div>
              <div class="int">
                 <input  class="ints" type="text" placeholder="选填" v-model="yaocode">
              </div>
            </div> -->
            
          </div>
        </div>
        <div class="tomub" @click="getlists()">
          立即升级
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
      index: '',
      yaocode:"",
      sex:"0",
      username:"",
      mobile:"",
      wxcode:"",
      region:"",
      yaocode:"",
      colors:'#333'
    }
  },
  components: {
    Radio,
    Group
  },
  methods: {
    changes(){
      console.log(this.sex)
    },
    getlists () {
      if(this.username ===''){
        this.$vux.toast.text('姓名不能为空')
      }else if(this.mobile ===''){
        this.$vux.toast.text('手机号不能为空')
      }else if(!/[0-9]{11}/.test(this.mobile)){
        this.$vux.toast.text('请输入正确的手机号')
      }
      // else if(this.wxcode ==''){
      //   this.$vux.toast.text('微信号不能为空')
      // } 
       else if(this.sex ==''){
        this.$vux.toast.text('性别不能为空')
      }else if(this.region ==''){
        this.$vux.toast.text('地区不能为空')
      }else{
        //amoy/user/apply-partner   合伙人
        this.$http.post('/amoy/user/apply-partner', {
          invite_code:this.yaocode,
          username:this.username,
          mobile:this.mobile,
          wxcode:this.wxcode,
          sex:this.sex,
          region:this.region
        }, true).then((res) => {
          if (res.code === 0) {
              this.$router.push('/my')
          }else{
          }
        })
      }
    },
    // 获取支付通道
  },
  mounted () {
  }
}
</script>
<style scoped lang="less">
  .partnership{
    .partnershipBox{
      background-image: url('../../assets/img/my/bj.png');
      -webkit-background-size: 100%;
      background-size: 100%;
      padding-top: 3rem;
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
          padding: 0.2rem 0;
          display: flex;
          flex-wrap: wrap;
          border-bottom: 1px solid #f4f4f4;
          .inttit{
            width: 1.5rem;
            text-align: left;
          }
          .int{
            margin-left: .1rem;
            .ints{
              background: #fff;
              font-size: 0.26rem;
              outline: none;
              border: none;
            }
            #man{
            }
            .issex{
              display: flex;
              flex-wrap: wrap;
              color: #000;
              div{
                height: .3rem;
                line-height: .3rem;
                 padding-top: .05rem;
                #intrad{
                  margin-right: .3rem;
                  margin-left: .1rem;
                  width: .3rem;
                  height: .3rem;
                  border-radius: 50%;
                  line-height: .3rem;
                  background: #666;
                }
              }
            }
          }
          img{
            max-width: 100%;
          }
        }
      }
    }
    .tomub{
      width: 4rem;
      height: 1rem;
      background: #fff;
      margin: 0 auto;
      text-align: center;
      line-height: 1rem;
       border-radius: 5px;
       font-size: .3rem;
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
