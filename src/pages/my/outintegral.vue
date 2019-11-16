<template>
  <div class="bankdetailed">
    <div class="containers">
      <ul class="bank_list_box">
        <li class="lists em">
          <div class="name  lt">接受地址</div>
          <div class="name_intes lt">
            <input  type="text" placeholder="请输入接受地址" v-model='address'>
          </div>
        </li>
        <li class="lists em">
          <div class="name lt">转出数量</div>
          <div class="name_int lt">
            <input type="text" placeholder="请输入转入数量" v-model='nums'>
          </div>
        </li>
        <li class="lists em">
          <div class="name lt">CSPC支付密码</div>
          <div class="name_int lt">
            <input type="password" placeholder="请输入支付密码" v-model='pwd'>
          </div>
        </li>
        <li class="lists em">
          <div class="name lt">备注</div>
          <div class="name_int lt">
            <input type="text" placeholder="请输入转出备注 " v-model='bz'>
          </div>
        </li>
      </ul>
    </div>
  <div class="tocore" @click="sub()">
    提交
  </div>
  </div>
</template>
<script>
export default {
  name:"outintegral",
  components: {

    },
  data(){
    return {
      address:"",
      nums:"",
      pwd:"",
      bz:"",

    }
  },
    methods:{
    showalert(){
       this.$store.commit('setshowrules',true)
    },
  //提交
    sub(){
      if(this.address.length <= 0){
        this.$vux.toast.text("接受地址不能为空")
      }else if(this.nums.length <= 0){
         this.$vux.toast.text("转入数量不能为空")
      }else if(this.pwd.length <= 0){
         this.$vux.toast.text("支付密码不能为空")
      }else{
        this.$http.post('/amoy/chain/transfer', {
        wallet_addr:this.address,
        amount:this.nums,
        password:this.pwd,
        remark:this.bz
      }, false, true).then((res) => {
        if (res.code === 0) {
          this.$vux.toast.text(res.msg)
          this.$router.push('/shoppingnum')
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
      }
    },
  },
  mounted(){
    
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
          .name_intes{
            width: 5rem;
            input{
              width: 5rem;
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
</style>



