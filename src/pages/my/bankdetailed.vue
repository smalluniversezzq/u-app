<template>
  <div class="bankdetailed">
    <div class="containers">
      <ul class="bank_list_box">
        <li class="lists em" v-for="(item,index) in data" :key="index">
          <div class="list_txt lt">
            <p class="list_title">{{item.remark}}</p>
            <p>订单号：{{item.orderId}}</p>
            <p class=" corend">{{item.created_at}}</p>
          </div>
          <div class="times rt">
            {{item.price}}
          </div>
        </li>
      </ul>
    </div>
    <div class="bosh">没有更多了~</div>
  </div>
</template>
<script>
export default {
  data(){
    return {
      data:"",
    }
  },
  methods:{
  //获取bankinfo
  getbankInfoList(){
    this.$http.post('amoy/bank/lately-list', {
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
    this. getbankInfoList()
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
              width: 5rem;
              height: .4rem;
              overflow: hidden;
            }
            .corend{
              font-size: .2rem;
              color: #b0b0b0;
              width: 5rem;
              overflow: hidden;
            }
          }
          .times{
            font-size: .3rem;
            color: #ff5846;
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


