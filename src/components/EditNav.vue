<template>
    <div class="EditNav">
      <div class="allBox"  :style="{background: data.bgColor}">
        <div class="allList" v-for="(item, index) of data.items" :key="index" @click="jumpLink(item.link)">
          <img v-lazy="item.img" alt="" :key="item.img">
          <span :style="{color: item.color}">{{item.text}}</span>
        </div>
      </div>
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
    </div>
</template>

<script>
export default {
  name: 'EditNav',
  components: {},
  props:['data','types','price','states'],
  data () {
    return {
        tobo: require('../assets/img/my/tobo.png'),
        toyun: require('../assets/img/my/toyun.png'),
    }
  },
  methods: {},
  computed: {
    w () {
      return ((1 / this.data.items.length).toFixed(4) * 100) + '%'
    },
  },
  created () {
  },
  mounthd(){
    console.log(this.states)
  }
}
</script>

<style scoped lang="less">
  .EditNav{
    margin-top: -1px;
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
  }
  .allBox{
    width: 100%;
    overflow: hidden;
    /*padding: .2rem 0 0;*/
    /*margin: .2rem 0;*/
    background: #fff;
    display: flex;
    .allList{
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      img{
        width: 0.8rem;
        height: 0.8rem;
        border-radius: 50%;
        margin-bottom: .05rem;
      }
      span{
        font-size: .24rem;
        word-break: keep-all;
      }
    }
  }
  .lt{float:left;}
  .rt{float:right;}
</style>
