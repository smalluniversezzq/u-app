<template>
  <div class="bankdetailed">
    <div class="outpwds"  v-show="outpwds === true" >点击发送验证码到注册手机号</div>
    <div class="containers">
      <ul class="bank_list_box">
        <!-- 这里有问题  type 分别代表什么 1显示  0不显示   2-->
          <!-- <li class="lists em" v-if="type=='1'">
          <div class="name lt">当前密码</div>
          <div class="name_int lt">
            <input type="password" placeholder="请输入当前支付密码" v-model="nowpwd">
          </div>
         </li> -->
           <!-- <li class="lists em" v-if="type=='2'">
          <div class="name lt">当前密码</div>
          <div class="name_int lt">
            <input type="password" placeholder="请输入当前支付密码" v-model="nowpwd">
          </div>
        </li>  -->
        <li class="lists em" v-if='nowpwds && type=="1"'>
          <div  class="name lt">当前密码</div>
          <div class="name_int lt">
            <input type="password"  placeholder="请输入当前支付密码" v-model="nowpwd">
          </div>
        </li>
        <li v-show="outpwds === true"  class="lists em">
          <!-- @change="codesms" -->
          <div class="label2">
            <input class="labelint" type="text" placeholder="请输入验证码"  v-model="smsCode">
            <span class="labelspan"  @click="handSmsCode">{{getCode}}</span>
          </div>
        </li>
        <li class="lists em">
          <div class="name lt">新密码</div>
          <div class="name_int lt">
            <input type="password" placeholder="请输入新密码"  v-model="pwd">
          </div>
        </li>
        <li class="lists em">
          <div class="name lt">确认新密码</div>
          <div class="name_int lt">
            <input type="password" placeholder="请再次输入新密码" v-model="newpwd">
          </div>
        </li>
      </ul>
    </div>
    <div class="outpwd" v-show="type==1"  @click="outpwd">重置密码</div>
  <div v-if="submits" class="tocore" @click="setpwd(type)">
    提交
  </div>
    <div v-if="btns" class="tocore" @click="subsmscode()">
    提交验证码
  </div>
  </div>
</template>
<script>
export default {
  name:"",
  components: {
    },
  data(){
    return {
      nowpwd:"",
      pwd:"",
      newpwd:"",
      type:"",
      outpwds:false,
      smsCode:"",
      timerFlag:false,
      getCode: '获取验证码',
      btns:false,
      submits:true,
      nowpwds:true,
    }
  },
  methods:{
    //设置密码
    setpwd(){
      if(this.pwd.length <=0){
          this.$vux.toast.text("新密码不能为空")
      }else if(this.newpwd.length <=0){
          this.$vux.toast.text("确认新密码不能为空")
      }else if(this.pwd !==this.newpwd){
          this.$vux.toast.text("两次输入的密码不一致")
      }else{     
          if(this.type == "1"){
            this.$http.post('/amoy/chain/set-paypassword', {
              type:this.type,
              password:this.pwd,
              re_password:this.newpwd,
              ori_password:this.nowpwd,
            }, false, true).then((res) => {
            if (res.code === 0) {
              this.data = res.data;
              this.$router.push("/shoppingnum")
            }else{
              this.$vux.toast.text(res.msg)
            }
          })
        }else if(this.type == "0"){
            this.$http.post('/amoy/chain/set-paypassword', {
              type:this.type,
              password:this.pwd,
              re_password:this.newpwd,
              ori_password:this.nowpwd,
            }, false, true).then((res) => {
            if (res.code === 0) {
                this.$vux.toast.text(res.msg)
                this.$router.push('/shoppingnum')
            }else{
              this.$vux.toast.text(res.msg)
            }
          })
        }
      }
    },
    handSmsCode () {
          this.btns = true
          this.submits = false
        if (this.timerFlag === false) {
          this.$http.post('/amoy/chain/send-sms', {
            // mobile: this.mobile,
            // type: 'register'
          }, true, true).then(res => {
            // this.invite = res.data.isNeedInvite !== '0'
            if (res.code === 0) {
              this.timerFlag = !this.timerFlag
              this.$vux.toast.text(res.msg)
              this.countDown(60)
            } else {
              this.$vux.toast.text(res.msg)
            }
          })
        } else {
          return ''
        }
    },
    //验证验证码  lllll
    subsmscode(){
      if(this.smsCode.length<=0){
        this.$vux.toast.text("验证码不能为空")
      }else{
        this.$http.post('/amoy/chain/verifycode', {
            sms_code:this.smsCode
          }, false, true).then((res) => {
          if (res.code === 0) {
              // this.nowpwd = res.data.password	
              // this.btns=false
              // this.submits=true
              // this.nowpwd= 12346598
            if(res.data.status == '0'){
              this.nowpwds = false
            }else if(res.data.status == '1'){
              this.nowpwd = res.data.password	
              this.btns=false
              this.submits=true
              this.nowpwd= res.data.password	
             
            }
          }else{
            this.$vux.toast.text(res.msg)
          }
        })
      }
    },
    countDown (time) {
      let count = time
      setTimeout(() => {
        count--
        if (time <= 0) {
          this.getCode = '获取验证码'
          this.timerFlag = false
        } else if (count !== time) {
          this.getCode = '重新获取' + count
          this.countDown(count)
        }
      }, 1000)
    },
    outpwd(){
      //忘记密码时 当前密码不显示
      this.outpwds = true;
      this.outpwdre = true;
      this.btns = true;
      this.submits = false;
      this.nowpwds =false

    }
  },
  mounted:function(){
    this.type = this.$route.query.state;
    console.log(this.type)
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
          min-height: 1rem;
          padding: 0 .2rem;
          border-bottom: 1px solid #eeeeee;
          line-height: 1rem;
          .name{
            width:1.8rem;
          }
          .name_int{
            input{
              background: #fff;
              font-size: 0.26rem;
              outline: none;
              border: none;
            }
          }
           .name_ints{
            textarea{
              width: 100%;
              background: #fff;
              font-size: 0.26rem;
              outline: none;
              border: none;
            }
          }
        }
     }
   } 
   .outpwd{
     text-align: right;
     padding-right: .2rem;
     margin-top: .2rem;
     color: #666;
   }
   .outpwds{
    width: 100%;
    height: .3rem;
    line-height: .3rem;
    background: #f63f37;
    text-align: center;
    color: #fff;
   }

.tocore{
      width: 5.22rem;
      height: .9rem;
      background:  linear-gradient(to right,#f97b65, #f63f37);
      margin: 1.3rem auto .5rem;
      text-align: center;
      line-height: .9rem; 
      font-size: .3rem;
      color: white;
      border-radius: .5rem;
    }
  }
  .lt{float: left;}
  .rt{float: right;}
   ul :nth-last-child(li){
      border:none;
    }
    .label2{
      background: url('../../assets/img/auth/password.png') no-repeat  0.2rem 0.35rem;
      background-size: .42rem .42rem;
      position: relative;
      .labelint{
        width: 6rem;
        border: none;
        background: rgba(0, 0, 0, 0);
        line-height: 0.6rem;
        margin: 0.25rem 0;
        font-size: 0.3rem;
        padding-left: 0.82rem;
      }
      .labelspan{
        font-size: 0.26rem;
        background: #585858;
        width: 1.7rem;
        height: 0.58rem;
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        color: #fff;
        border-radius: 0.1rem;
        position: absolute;
        right: 0;
        top: 0.25rem;
      }
    }
</style>



