<template>
  <div class="bankdetailed">
    <div class="containers">
      <ul class="bank_list_box">
        <li class="lists em">
          <div class="name lt">姓名:</div>
          <div class="name_int lt">
            <input type="text" placeholder="请输入真实姓名">
          </div>
        </li>
        <li class="lists em">
          <div class="name lt">电话:</div>
          <div class="name_int lt">
            <input type="text" placeholder="请输入有效电话号码">
          </div>
        </li>
        <li class="lists em">
         <group>
            <x-address title="所在地区：" v-model="addressValue" raw-value :list="addressData" value-text-align="left"></x-address>
         </group>
        </li>
        <li class="lists em">
          <div class="name ">详情地址：</div>
          <div class="name_ints ">
            <textarea placeholder="如道路、门牌号、小区、楼栋号、单元室等"></textarea>
          </div>
        </li>
      </ul>
    </div>
  <div class="tocore" @click="jumpTo('/signIn')">
    前往任务中心
  </div>
  </div>
</template>
<script>
  import { GroupTitle, Group, Cell, XInput, Selector, PopupPicker, Datetime, XNumber, ChinaAddressData, XAddress, XTextarea, XSwitch } from 'vux'
export default {
      components: {
      Group,
      GroupTitle,
      Cell,
      XInput,
      Selector,
      PopupPicker,
      XAddress,
      Datetime,
      XNumber,
      XTextarea,
      XSwitch
    },
  data(){
    return {
      addressData: ChinaAddressData,
      addressValue: ['北京市', '市辖区', '东城区'],
    }
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
            width:1.2rem;
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
   .lt{float: left;}
  .rt{float: right;}
  ul :nth-last-child(li){
      border:none;
  }
// {
// background:#ff0000;
// }

</style>
<style>
/* .vux-popup-picker-value{ */
  /* color:#999!important;
}
.vux-no-group-title{
  margin-top: 0 !important;
}
.weui-cells:after {
  border-bottom:none  !important;
}
.weui-cells::before {
  border-bottom:none  !important;
}
.weui-cells:before{
  border-bottom:none  !important;
  border-top:none!important;
}
.weui-cell{
  padding: 0 !important;
}
.weui-cell_access{
  line-height: 1rem;
  font-size: .2rem;
}
.weui-cell__ft{
  display: none;
}
.vux-popup-header-right{
  color: #000 !important;
}
.weui-cell__hd{
  width: 1.2rem !important;
}
.weui-label{

}
.vux-popup-picker-select-box{
  width: 3rem !important;
}
.weui-cell_access:active{}
.weui-cells{
    background-color:#fff !important;
} */

</style>



