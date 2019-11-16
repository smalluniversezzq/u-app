<template>
    <div class="GoodsList">
      <!-- <div :class="{goods_list_2 : !styles, goods_list_1 : styles}"> -->
        <!-- <ul class="goods_ul">
          <li class="goods_li" v-for="item of list" :key="item.origin_id" @click="detail(item)">
            <div class="preview">
              <img v-lazy="item.thumb" alt="" :key="item.thumb">
              <img src="../assets/img/home/video.png" alt="" class="video" v-if="item.video_url && showVideo"  @click.stop="openVideo(item)">
              <p class="commission_money" v-if="item.commission_money">分享赚：{{item.commission_money}}</p>
            </div>
            <div class="con_right">
              <p class="brand">
                <img src="../assets/img/home/tblogo.png" alt="" v-if="Number(item.type) === 11">
                <img src="../assets/img/home/tmlogo.png" alt="" v-else-if="Number(item.type) === 12">
                <img src="../assets/img/home/jdlogo.png" alt="" v-else-if="Number(item.type) === 21">
                <img src="../assets/img/home/pddlogo.png" alt="" v-else-if="Number(item.type) === 31">
                {{item.title}}
              </p>
              <p class="listinfo">{{item.description}}</p>
              <div class="original"><p>原价:&nbsp;&nbsp;<del>￥{{item.zk_final_price}}</del></p><i>已售: {{item.volume}}</i></div>
              <div class="shoptitle">{{item.shop.title}}</div>
              <div class="price"><span class="later" :style="{background: 'linear-gradient('+$store.state.global.theme.deg+'deg,' +$store.state.global.theme.mainColor+','+$store.state.global.theme.secondColor+')'}">券后价</span><p :style="{color: $store.state.global.theme.mainColor}">￥<span>{{item.coupon_price}}</span></p><b :style="{background: 'linear-gradient('+$store.state.global.theme.deg+'deg,' +$store.state.global.theme.mainColor+','+$store.state.global.theme.secondColor+')'}">{{item.coupon_money || 0}}元券</b></div>
              <div class="up_gain" v-if="$bizId !== 1 && item.level_commission_money && item.level_commission_money != 0">
                <span v-if="item.commission_money">预估赚:￥{{item.commission_money}}</span>
                <span v-if="item.level_commission_money && item.level_commission_money != 0 && (item.commission_money != item.level_commission_money)">升级赚:￥{{item.level_commission_money}}</span>
              </div>
            </div>
          </li>
        </ul> -->
      <div class="goodlist_box"  >
        <div class="goods_li" v-for="(item,index) of list" :key="index"  :style="(index%2 ==0) ?'margin-right:.2rem':'margin-right:0rem'"  @click="detail(item)">
          <div class="preview">
            <img :src="item.thumb"  class="video" >
            <p class="commission_money" >分享赚：{{item.commission_money}}</p> 
          </div>
          <div class="con_right">
            <div class="con_quan_box">
              <div class="quan lt">{{item.coupon_money}}元券</div>
              <div class="shouyi rt">预估收益￥{{item.shouyi}}</div>
            </div>
            <div class="goodstitle">
              <p class="brand lt">
                <img src="../assets/img/home/tblogo.png" alt="" v-if="Number(item.type) === 11">
                <img src="../assets/img/home/tmlogo.png" alt="" v-else-if="Number(item.type) === 12">
                <img src="../assets/img/home/jdlogo.png" alt="" v-else-if="Number(item.type) === 21">
                <img src="../assets/img/home/pddlogo.png" alt="" v-else-if="Number(item.type) === 31">
              </p>
              <div class="title lt">{{item.title}}</div>
            </div>
            <div class="price">
              <span class="later" >券后价:￥</span>
              <span class="price_num">{{item.coupon_price}}</span>
            </div>
            <div class="prices_box">
              <div class="lt prices_box_lt">
                ￥{{item.origin_price}}
              </div>
              <div class="rt prices_box_rt">已售{{item.volume}}万</div>
            </div>
 
          </div>
      </div>
    </div>
        <div id="empty"></div>
      </div>
</template>

<script>
export default {
  name: 'GoodsList',
  components: {},
  props: ['list','styles',],
  data () {
    return {}
  },
  methods: {
    detail (item) {
      this.$emit('detail', item)
    },
    openVideo (item) {
      this.$emit('openVideo', item)
    }
  },
  mounted () {
  }
}
</script>
<style scoped lang="less">
  .shoptitle{
    text-align: left;
    color: #999;
    font-size: 0.22rem;
    line-height: 0.3rem;
    overflow: hidden;
}
    .goodlist_box{
      margin: .2rem;
      display: flex;
      flex-wrap: wrap;
      .goods_li{
        width: 3.4rem;
        height: 5.5rem;
        border-radius: .15rem;
        background: #fff;
        overflow: hidden;
        margin-bottom: .2rem;
        .preview{
          width: 100%;
          height: 3.4rem;
          position: relative;
          border-radius: .15rem;
          overflow: hidden;
          .commission_money{
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: .4rem;
            text-align: center;
            line-height: .4rem;
            color: #fff;
            background: rgba(0, 0, 0, 0.4);
          }
        }
        .con_right{
          padding: .1rem;
          box-sizing: border-box;
          .con_quan_box{
            clear: both;
            height: .36rem;
            // display: flex;
            // flex-wrap: wrap;
            font-size: .3rem;
            .quan{
              width: 1.1rem;
              height: 0.36rem;
              line-height: 0.36rem;
              text-align: center;
               background:  linear-gradient(to right,#ff5949, #fe8e76);
              border-radius: 0.1rem;
              color: white;
              position: relative;
              font-size: 0.25rem;
              overflow: hidden;
            }
            .quan:before{
              content: '';
              position: absolute;
              left: -0.05rem;
              top: 0.14rem;
              height: 0.1rem;
              width: 0.1rem;
              border-radius: 100%;
              background: #fff;
            }
            .quan:after{
              content: '';
              position: absolute;
              right: -0.05rem;
              top: 0.14rem;
              height: 0.1rem;
              width: 0.1rem;
              border-radius: 100%;
              background: #fff;
            }
            .shouyi{
              width: 2rem;
              height: .36rem;
              line-height: .36rem;
              text-align: center;
              font-size: .26rem; 
              color:#ff5848;
              background: #fff6f5;
              border-radius: .015rem;
            }
          }
          .goodstitle{
            margin-top: .15rem; 
            .brand{
                width: .3rem;
                height: .3rem;
                border-radius: .015rem;
                overflow: hidden;
                margin-right: .1rem;
            }
            .title{
              width: 2.8rem;
              display: inline-block;
              font-size: 0.28rem;
              line-height: 0.33rem;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;
            }
          }
          .price{
            color:#fd5747;
            .later{
              font-size: .25rem;
            }
            .price_num{
              font-size: .30rem;
            }
          }
          .prices_box{
             color: #b8b8b8;
            .prices_box_lt{
              text-align: left;
             text-decoration:line-through
            }
            .prices_box_rt{
               text-align: right;
            }
          }
        }
      }
    }
        .lt{float: left;}
  .rt{float: right;}
</style>
