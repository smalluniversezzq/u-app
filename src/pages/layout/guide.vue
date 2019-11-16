<template>
    <div class="guide" :style="{height:heights}">
      <!-- <swiper height="100%" :show-dots="dots">
        <swiper-item class="swiper-demo-img" v-for="(item, index) of list" :key="index"><img :src="item"  @click="change(index)"></swiper-item>
      </swiper> -->
      <div class="guide_box" :style="{height:heights}">
        <div class="pic" v-for="(item, index) of data" :key="index"  :style="{zIndex:indexs[index],height:heights}"  >
          <div class="guipic" :style="{height:heights}">
            <img :style="{height:heights}"  :src="item" alt="">
          </div>
          <div class="close"  @click="close()">
          </div>
          <div class="next" @click="next()">
          </div>
        </div>
      </div>
    </div>
</template>
<script>
// 引导66666666
import {Swiper, SwiperItem} from 'vux'
import * as utils from '../../utils'
export default {
  name: 'guide',
  components: {
    Swiper, SwiperItem
  },
  data () {
    return {
      dots: false,
      list: utils.storage.get('guideList'),
      data:"",
      indexs:[
        10,9,8,7,6,5,4,3,2,1
      ],
      bos:"",
      heights:"",
    }
  },
  methods: {
      next(){
       this.data = this.data.slice(1)
       if(this.data.length == 0){
         utils.storage.set('Uquanyun', 'first')
         this.$router.push('/home')
       }
    },
    close(){
       utils.storage.set('Uquanyun', 'first')
       this.$router.push('/home')
    },
    // change (index) {
    //   if (index + 1 === this.list.length) {
    //     utils.storage.set('Uquanyun', 'first')
    //     this.$router.replace('/home')
    //   }
    // },
    tobootpage(){
    this.$http.get('/amoy/app/guide', {
      }, true).then((res) => {
      if (res.code === 0) {
        this.data = res.data
      }else{
      }
    })
  },
  },
  mounted () {
    this.tobootpage()
    window.canRightSlipBack = false
    this.heights = document.body.clientHeight + "px";
    console.log(this.list)
    // setTimeout(() => {
    //   api.removeLaunchView({
    //     animation: {
    //       type: 'fade',
    //       duration: 500
    //     }
    //   })
    // }, 5000)
  },
  destroyed(){
     window.canRightSlipBack = true
  }
}
</script>

<style lang="less">
  .guide{
    .vux-slider > .vux-indicator, .vux-slider .vux-indicator-right{
      width: 100%;
      left: 0;
      right: 0;
      text-align: center;
      a{
        float: none !important;
      }
    }
  }
</style>
<style scoped lang="less">
.guide{
  height: 100%;
  width: 100%;
  .guide_box{
     height: 100%;
    width: 100%;
      position: relative;
    .pic{
       height: 100%;
        width: 100%;
        position: absolute;
        top: 0;
        left: 0;
        overflow: hidden;
      .guipic{
        height: 100%;
        width: 100%;
        overflow: hidden;
        }
      .close{
        width: 50%;
        height: .6rem;
        position: absolute;
        right: 0;
        top: .4rem;
        background: rgba(250, 250, 250, 0);
        z-index: 99;
      }
      .next{
        position: absolute;
        width: 70%;
        height: 10rem;
        top: 5rem;
        background: rgba(250, 250, 250, 0);
        z-index: 99;
      }
    }
  }
  .vux-slider{
    height: 100%;
    width: 100%;
    .vux-swiper-item{
      width: 100%;
      height: 100%;
      img{
        width: 100%;
        height: 100%;
      }
    }
  }
}
</style>
