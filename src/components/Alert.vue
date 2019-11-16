<template>
    <div v-show="$store.state.global.showrules === true"  id="contactAlert">
      <div class="contactAlerts">
        <div class="em">
          <div class="title">
            收货信息
            </div>
            <div class="close" @click="close()"> X</div>
        </div>
        <div class="content" >
          <div class="userinfo">
              <span class='username'>齐天大圣</span>
              <span>15312345678</span>
          </div>
          <div class="address">
              河南省郑州市商务外环路绿地峰会天下17楼1705室
          </div>
        </div>
      </div>
    </div>
</template>
<script>
import * as utils from '../utils'
export default {
    name:"alert",
    props:[],
    data(){
      return {
    
      }
    },
    methods:{
    close(){
        this.$store.commit('setshowrules',false)
      }
    },
    mounted: function(){
    },
    updated:function(){
    }
}
</script>
<style lang="less" scoped>
    #contactAlert{
        width: 100%;
        height: 100%;
        background: rgba(76, 76, 76, .5);
        top: 0;
        left: 0;
        position: absolute;
        z-index: 1000;
    }
    .contactAlerts{
        position: absolute;
        width:6rem;
        min-height:2.9rem;
        top: 4rem;
        left:50%;
        margin-left: -3rem;
        background: #fff;
        border-radius: .15rem;
        overflow: hidden;
        .em{
          height: 1rem;
          border-bottom: 1px solid #eeeeee;
          position: relative;
          .title{
            height: 1rem;
            line-height: 1rem;
            text-align: center;

          }
          .close{
            position: absolute;
            top: .35rem;
            right: .35rem;
            // background:rgba(red, green, blue, 0);
          }
        }
        .content{
          padding: .3rem;
          box-sizing: border-box;
          .userinfo{
            .username{
              margin-right: .3rem;
            }
          }
        }
      }
</style>


