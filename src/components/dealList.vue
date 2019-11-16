<template>
  <div class="tb">
    <div class="em" v-if="state == 'tb'" style="background:#fe796a">
      <div class="obtain lt">
       淘宝成交预估收入：{{data.forecast_income}}元
      </div>
      <div class="nums rt">
        订单数:{{data.order_num}}
      </div>
    </div>
       <div class="em" v-if="state == 'pd'"  style="background:#e2599e">
      <div class="obtain lt">
        拼多多成交预估收入：{{data.forecast_income}}元
      </div>
      <div class="nums rt">
        订单数:{{data.order_num}}
      </div>
    </div>
       <div class="em" v-if="state == 'jd'"  style="background:#db5270">
      <div class="obtain lt">
       京东成交预估收入：{{data.forecast_income}}元
      </div>
      <div class="nums rt">
        订单数:{{data.order_num}}
      </div>
    </div>
    <ul class="commissionlist_box"   v-if="state == 'tb'">
      <!-- <li class="commission_list" > -->
      <li class="commission_list" >
          <p>{{"自产预估佣金"}}</p>
          <div class="money">
            {{data.my_forecast_income}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"直属团队预估佣金"}}</p>
          <div class="money">
            {{data.team_forecast_income}}
          </div>
      </li>
         <li class="commission_list" style="borderRight:none">
          <p>{{"非直属团队预估佣金"}}</p>
          <div class="money">
            {{data.unteam_forecast_income}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"自产订单数量"}}</p>
          <div class="money">
            {{data.my_order_num}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"直属团队订单数量"}}</p>
          <div class="money">
            {{data.team_order_num}}
          </div>
      </li>
         <li class="commission_list" style="borderRight:none">
          <p>{{"非直属团队订单数量"}}</p>
          <div class="money">
            {{data.unteam_order_num}}
          </div>
      </li>
    </ul>
      <ul class="commissionlist_box"  v-if="state == 'pd'">
      <!-- <li class="commission_list" > -->
      <li class="commission_list" >
          <p>{{"自产预估佣金"}}</p>
          <div class="money">
            {{data.my_forecast_income}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"直属团队预估佣金"}}</p>
          <div class="money">
            {{data.team_forecast_income}}
          </div>
      </li>
         <li class="commission_list" style="borderRight:none">
          <p>{{"非直属团队预估佣金"}}</p>
          <div class="money">
            {{data.unteam_forecast_income}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"自产订单数量"}}</p>
          <div class="money">
            {{data.my_order_num}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"直属团队订单数量"}}</p>
          <div class="money">
            {{data.team_order_num}}
          </div>
      </li>
         <li class="commission_list" style="borderRight:none">
          <p>{{"非直属团队订单数量"}}</p>
          <div class="money">
            {{data.unteam_order_num}}
          </div>
      </li>
    </ul >
      <ul class="commissionlist_box"  v-if="state == 'jd'">
      <!-- <li class="commission_list" > -->
      <li class="commission_list" >
          <p>{{"自产预估佣金"}}</p>
          <div class="money">
            {{data.my_forecast_income}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"直属团队预估佣金"}}</p>
          <div class="money">
            {{data.team_forecast_income}}
          </div>
      </li>
         <li class="commission_list" style="borderRight:none">
          <p>{{"非直属团队预估佣金"}}</p>
          <div class="money">
            {{data.unteam_forecast_income}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"自产订单数量"}}</p>
          <div class="money">
            {{data.my_order_num}}
          </div>
      </li>
         <li class="commission_list" >
          <p>{{"直属团队订单数量"}}</p>
          <div class="money">
            {{data.team_order_num}}
          </div>
      </li>
         <li class="commission_list" style="borderRight:none">
          <p>{{"非直属团队订单数量"}}</p>
          <div class="money">
            {{data.unteam_order_num}}
          </div>
      </li>
    </ul>
</div>
</template>
<script>
export default {
  name:"dealList",
  props:['data','state'],
  data(){
    return {
      list:[
      ],
    }
  }
}
</script>
<style scoped lang='less'>
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
        display: flex;
        flex-wrap: wrap;
        .obtain{
          width: 60%;
          text-align: left;
        }
        .nums{
          width: 40%;
          text-align: right;
        }
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
          font-size: .2rem;
          p{
            color: #888888;
          }
          .money{
            font-size: .3rem;
            font-weight: 800;
            text-align: left;
            color:#3e3a39;
          }
        }
      }
</style>


