<template>
  <div class="tool" :style="{backgroundColor: data.bgColor}">
      <div v-if="types === 2" class="banks"  @click="jumpTo('/bank')">
        <div class="ban lt">积分银行</div>
        <div class="num rt">余额{{price}}</div>
      </div>
      <!-- partnership -->
      <div v-if="types === 2">
        <!-- <div  class="tobo"  @click="jumpTo('/partnership?state=' + states)">
          <img :src="tobo" alt="">
        </div> -->
         <div v-if="states === '普通会员'" class="tobo"  @click="jumpTo('/partnerships?state=' + states)">
          <img :src="tobo" alt="">
        </div>
        <div v-if="states === '合伙人'" class="tobo"  @click="jumpTo('/partnership?state=' + states)">
          <img :src="tobo" alt="">
        </div>
        <div v-if="states === '运营商'" class="tobo"  @click="jumpTo('/partnership?state=' + states)">
          <img :src="tobo" alt="">
        </div>
      </div>
      <div v-if="types === 2">
        <div v-if="states === '普通会员'" class="toyun"  @click="jumpTo('/partnerships?state=' + states)">
          <img :src="toyun" alt="">
        </div>
        <div v-if="states === '合伙人'" class="toyun"  @click="jumpTo('/partnership?state=' + states)">
          <img :src="toyun" alt="">
        </div>
        <div v-if="states === '运营商'" class="toyun"  @click="jumpTo('/partnership?state=' + states)">
          <img :src="toyun" alt="">
        </div>
      </div>

    <div class="headers" :style="{display: data.display}">{{data.title}}</div>
    <div class="option">
      <div v-for="(item, index) of data.items" :key="index" @click="jumpLink(item.link)">
        <div class="imgBox"><img :src="item.img" alt=""></div>
        <div :style="{color: item.color}">{{item.text}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EditNavGroup',
    props:['data','types','price','states'],
  components: {},
  data () {
    return {
      tobo: require('../assets/img/my/tobo.png'),
      toyun: require('../assets/img/my/toyun.png'),
    }
  },
  methods: {}
}
</script>

<style scoped lang="less">
  .tool{
    margin-top: -1px;
    border-radius: 10px;
    background: #fff;
        .banks{
      height: 1rem;
      line-height: 1rem;
      text-align: center;
      border-radius: .15rem;
      background: #fff;
      padding: 0 .2rem;
      margin:.2rem 0;
      .num{
        color: #fe5946;
      }
      .ban{
         color: #000;
      }
    }
    .toyun{
      
    }
    .option{
      width: 100%;
      padding: 0.26rem 0 0;
      box-sizing: border-box;
      /*border-radius: 10px;*/
      flex-wrap: wrap;
      overflow: hidden;
      >div{
        margin-bottom: .4rem;
        float: left;
        width: 25%;
        text-align: center;
        font-size: 0.24rem;
        color: #666;
        div:nth-child(1){
          width: 0.36rem;
          height: 0.36rem;
          margin: 0 auto;
          margin-bottom: 0.14rem;
          img{
            width: 100%;
            height: 100%;
          }
        }
      }
    }
    .headers{
      float: left;
      line-height: 1rem;
      font-size: 0.3rem;
      width: 100%;
      padding-left: 0.2rem;
      position: relative;
      box-sizing: border-box;
      text-align: left !important;
    }
    .headers:before{
      content: '';
      position: absolute;
      width: 100%;
      height: 1px;
      background-color: #eeeeee;
      bottom: 0;
      left: 0;
    }
  }
  .lt{float:left;}
  .rt{float:right;}
</style>
