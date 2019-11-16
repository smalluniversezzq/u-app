<template>
  <div id="Video" class="videoIndex" >
    <div class="headerConBox">
      <div class="header">
        <i class="iconfont icon-back" @click="goBack()"></i>
        <p class="title">共享影视</p>
      </div>
    </div>
    <div class="bannerSwiper">
      <div class="successNav">
        <swiper :options="swiperOption" class="swiper">
          <swiper-slide v-for="(banner, index) in banners" :key="'swiper'+index">
            <img :src="banner.image" @click="jumpTo('/videoPlay?id='+banner.id)">
          </swiper-slide>
          <div class="swiper-pagination"  slot="pagination"></div>
        </swiper>
      </div>
    </div>
    <div class="searchBox">
      <div class="box">
        <div class="inputBox">
          <input type="text" v-model="search" placeholder="请输入关键词">
          <img src="../../assets/img/video/sousuo.png" alt="">
        </div>
        <span @click="jumpTo('/videoSearch?value=' + search+'&type=0')">搜索</span>
      </div>
    </div>
    <div class="cueCard">
      <p class="cardTitle">您当前的观影特权已激活</p>
      <div class="gradeDesc">
        <p>
          <span class="title">
            激活方式：
          </span>
          <span class="content">
            等级特权
          </span>
        </p>
        <p>
          <span class="title">
            到期时间：
          </span>
          <span class="content">
            永久有效
          </span>
        </p>
      </div>
    </div>
    <div class="videoPlatform">
      <div class="videoTitleBox">
        <img class="titleIcon" src="../../assets/img/video/VIP.png" alt="">
        <p class="titleText">全网VIP</p>
        <p class="titleDesc">主流视频平台VIP视频免费看</p>
      </div>
      <div class="vodeoList">
        <div class="logoItem" v-for="(item,index) in logoList" :key="'videoLogo'+index" @click="watchVideo(item)">
          <div class="logoBox">
            <img :src="item.src" alt="">
          </div>
          <p class="logoName">
            {{item.title}}
          </p>
        </div>
        <div class="logoItemBut" v-show="allLogo" @click="showAllLogoList()">
          <div class="logoBox">
            <img src="../../assets/img/video/all.png" alt="">
          </div>
          <p class="logoName">展开<img src="../../assets/img/video/zhankai.png" alt=""></p>
        </div>
      </div>
    </div>
    <div class="videoBox" v-show="filmList">
      <div class="videoTitle">
        <i class="titleIcon"></i>
        <p class="title">热门电影</p>
        <div class="moreBox"><sapn class="moreText" @click="jumpTo('/videoRecommend?type=0&title=热门电影')">更多</sapn><img class="moreIcon" src="../../assets/img/video/gengduo.png" alt=""></div>
      </div>
      <div class="videoItemBox">
        <div class="videoItem" v-for="(item, index) in filmList" :key="'film'+index" @click="jumpTo('/videoPlay?id='+item.id)">
          <div class="videoImgBox">
            <!-- <img class="videoImg" src="../../../static/img/under/11.png" alt=""> -->
            <div class="videoImg" :style="{backgroundImage: 'url(' + item.image + ')'}"></div>
          </div>
          <p class="videoName">{{item.title}}</p>
        </div>
      </div>
    </div>
    <div class="videoBox" v-show="tvList">
      <div class="videoTitle">
        <i class="titleIcon"></i>
        <p class="title">热门电视</p>
        <div class="moreBox"><sapn class="moreText" @click="jumpTo('/videoRecommend?type=1&title=热门电视')">更多</sapn><img class="moreIcon" src="../../assets/img/video/gengduo.png" alt=""></div>
      </div>
      <div class="videoItemBox">
        <div class="videoItem" v-for="(item, index) in tvList" :key="'tv'+index" @click="jumpTo('/videoPlay?id='+item.id)">
          <div class="videoImgBox">
            <!-- <img class="videoImg" src="../../../static/img/under/11.png" alt=""> -->
            <div class="videoImg" :style="{backgroundImage: 'url(' + item.image + ')'}"></div>
          </div>
          <p class="videoName">{{item.title}}</p>
        </div>
      </div>
    </div>
    <div class="videoBox" v-show="varietyList">
      <div class="videoTitle">
        <i class="titleIcon"></i>
        <p class="title">热门综艺</p>
        <div class="moreBox"><sapn class="moreText" @click="jumpTo('/videoRecommend?type=2&title=热门综艺')">更多</sapn><img class="moreIcon" src="../../assets/img/video/gengduo.png" alt=""></div>
      </div>
      <div class="videoItemBox">
        <div class="videoItem" v-for="(item, index) in varietyList" :key="'variety'+index" @click="jumpTo('/videoPlay?id='+item.id)">
          <div class="videoImgBox">
            <!-- <img class="videoImg" src="../../../static/img/under/11.png" alt=""> -->
            <div class="videoImg" :style="{backgroundImage: 'url(' + item.image + ')'}"></div>
          </div>
          <p class="videoName">{{item.title}}</p>
        </div>
      </div>
    </div>
    <div class="videoBox" v-show="cartoonList">
      <div class="videoTitle">
        <i class="titleIcon"></i>
        <p class="title">热门动漫</p>
        <div class="moreBox"><sapn class="moreText" @click="jumpTo('/videoRecommend?type=3&title=热门动漫')">更多</sapn><img class="moreIcon" src="../../assets/img/video/gengduo.png" alt=""></div>
      </div>
      <div class="videoItemBox">
        <div class="videoItem" v-for="(item, index) in cartoonList" :key="'cartoon'+index" @click="jumpTo('/videoPlay?id='+item.id)">
          <div class="videoImgBox">
            <!-- <img class="videoImg" src="../../../static/img/under/11.png" alt=""> -->
            <div class="videoImg" :style="{backgroundImage: 'url(' + item.image + ')'}"></div>
          </div>
          <p class="videoName">{{item.title}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {swiper, swiperSlide} from 'vue-awesome-swiper'
  import 'swiper/dist/css/swiper.css'
export default {
  name: 'VideoHome',
  components: {
    swiper, swiperSlide
  },
  data () {
    return {
      search: '',
      banners: [],
      swiperOption: {
          pagination: { el: ".swiper-pagination"},
          paginationClickable :true,
          loop: false,
          autoplay: {
          delay: 1500,
          stopOnLastSlide: false,
          disableOnInteraction: false,
          }
      },
      videoList: [{src: require('../../assets/img/home/video/tx.png'), title: '腾讯视频', path: 'https://v.qq.com/'},
        {src: require('../../assets/img/home/video/aqy.png'), title: '爱奇艺', path: 'https://www.iqiyi.com/'},
        // {src: require('../../assets/img/home/video/guangaotoufang.png'), title: '广告投放', path: '/ads'},
        {src: require('../../assets/img/home/video/mgu.png'), title: '咪咕视频', path: 'http://m.miguvideo.com/'},
        {src: require('../../assets/img/home/video/fx.png'), title: '风行视频', path: 'http://m.fun.tv/'},
        {src: require('../../assets/img/home/video/bf.png'), title: '暴风影音', path: 'http://m.baofeng.com/'},
        {src: require('../../assets/img/home/video/sh.png'), title: '搜狐视频', path: 'https://m.tv.sohu.com/'},
        {src: require('../../assets/img/home/video/mg.png'), title: '芒果TV', path: 'https://m.mgtv.com/'},
        {src: require('../../assets/img/home/video/1905.png'), title: '1905', path: 'http://m.1905.com/'},
        {src: require('../../assets/img/home/video/ls.png'), title: '乐视视频', path: 'http://m.le.com/'},
        {src: require('../../assets/img/home/video/yk.png'), title: '优酷视频', path: 'http://youku.com/'},
        {src: require('../../assets/img/home/video/pptv.png'), title: 'PPTV', path: 'http://m.pptv.com'}],
        logoList: [],
        allLogo: true,
        filmList: '',/* 电影列表 */
        cartoonList: '',/* 动漫 */
        tvList: '',/* 电视 */
        varietyList: ''/*, 综艺 */
    }
  },
  computed:{
  },
  methods: {
    videoLogo() {
      var that = this
      if(that.videoList.length > 7) {
        for(var i = 0; i < 7; i++) {
          that.logoList.push(that.videoList[i])
        }
      } else {
        that.logoList = that.videoList
      }
    },
    getInfo() {
      this.$http.get('/amoy/share-video/video-list', {page: 1,type: 1}, true).then(res => {
        if(res.code == 0){
          this.banners = res.data.list
          this.getFilmList()
        }
      })
    },
    getFilmList() {
      var that = this
      that.$http.get('/amoy/share-video/video-list', {page: 0,type: 1}, true).then(resone => {
        if(resone.code == 0){
            that.filmList = resone.data.list
            that.getTvList()
        }
      })
    },
    getTvList() {
      var that = this
            that.$http.get('/amoy/share-video/video-list', {page: 0,type: 2}, true).then(restwo => {
              if(restwo.code == 0){
                  that.tvList = restwo.data.list
                  that.getVarietyList()
              }
            })
    },
    getVarietyList() {
      var that = this
            that.$http.get('/amoy/share-video/video-list', {page: 0,type: 3}, true).then(resthree => {
              if(resthree.code == 0){
                  that.varietyList = resthree.data.list
                  that.getCartoonList()
              }
            })
    },
    getCartoonList() {
      var that = this
            that.$http.get('/amoy/share-video/video-list', {page: 0,type: 4}, true).then(resfour => {
              if(resfour.code == 0){
                  that.cartoonList = resfour.data.list
              }
            })
    },
    showAllLogoList() {
      var that = this
      that.logoList = that.videoList
      that.allLogo = false
    },
    watchVideo (item) {
      this.$router.push('/videoDetail?url=' + item.path + '&title=' + item.title)
    }
  },
  mounted () {
    var that = this
    that.videoLogo()
    that.getInfo()
  }
}
</script>
<style scoped lang="less">
  /deep/.swiper-pagination-bullet{
    width: 0.21rem;
    height: 0.08rem;
    border-radius: 0.04rem;
    background: #191919;
    opacity: 1;
  }
  /deep/.swiper-pagination-bullet-active{
    width: 0.21rem;
    height: 0.08rem;
    background: #F7F7F7;
  }
  /deep/.swiper-pagination-bullet{
    margin: 0 10px;
  }
  .videoIndex{
    font-family:PingFang-SC-Regular;
    // position: relative;
    overflow: hidden;
    background: #fff;
    .headerConBox{
      position: relative;
      overflow: hidden;
      width: 100%;
      .header{
        position: relative;
        width: 100%;
        height: 0.83rem;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #191919;
        .icon-back{
          position: absolute;
          font-size: 0.4rem;
          font-weight: bold;
          left: 0.31rem;
          z-index: 3;
        }
        .title{
          position: relative;
          width: 100%;
          font-family:PingFang-SC-Medium;
          font-size: 0.34rem;
          font-weight: bold;
          line-height: 0.83rem;
          text-align: center;
        }
      }
    }
    .bannerSwiper{
      .successNav{
        width: 100%;
      }
    }
    .searchBox{
      .box{
        position: relative;
        margin: auto;
        margin-top: 0.32rem;
        width: 6.9rem;
        height: 0.62rem;
        box-shadow:0px 0.02rem 0.02rem 0px rgba(247,247,247,1);
        background:rgba(255,255,255,1);
        border-radius: .1rem;
        .inputBox{
          position: absolute;
          overflow: hidden;
          width: 5.73rem;
          height: 0.62rem;
          border-top: 0.01rem solid rgba(238,238,238,1);
          border-left: 0.01rem solid rgba(238,238,238,1);
          border-bottom: 0.01rem solid rgba(238,238,238,1);
          border-top-left-radius: .1rem;
          border-bottom-left-radius: 0.1rem;
          input{
            position: absolute;
            left: 0;
            top: 0;
            outline: none;
            border: none;
            height: 0.62rem;
            width: 5.73rem;
            box-sizing: border-box;
            border-bottom-left-radius: .1rem;
            border-top-left-radius: .1rem;
            color: #999999;
            padding-left: .63rem;
            box-sizing: border-box;
            font-size: .26rem;
          }
          img{
            position: absolute;
            top: .17rem;
            left:.16rem;
            width: .29rem;
            height: .29rem;
          }
        }
        span{
          display: inline-block;
          position: absolute;
          right: 0;
          width: 1.17rem;
          height: 0.66rem;
          line-height: 0.64rem;
          text-align: center;
          font-size: .32rem;
          background: linear-gradient(-9deg,rgba(250,152,144,1),rgba(254,116,129,1),rgba(254,81,105,1));
          color: #fff;
          border-bottom-right-radius: .1rem;
          border-top-right-radius: .1rem;
        }
      }
    }
    .cueCard{
      position: relative;
      overflow: hidden;
      width: 6.88rem;
      box-sizing: border-box;
      margin: auto;
      margin-top: 0.45rem;
      padding-left: 0.2rem;
      padding-right: 0.2rem;
      padding-top: 0.34rem;
      padding-bottom: 0.34rem;
      background:linear-gradient(50deg,rgba(249,231,198,1),rgba(245,222,176,1),rgba(241,213,157,1));
      box-shadow:0px 0.02rem 0.06rem 0px rgba(201,142,73,0.5);
      border-radius:0.1rem;
      .cardTitle{
        width: 100%;
        margin-bottom: 0.3rem;
        box-sizing: border-box;
        font-family:PingFang-SC-Medium;
        font-size: 0.3rem;
        font-weight: 500;
        line-height: 0.3rem;
        color: #644529;
      }
      .gradeDesc{
        width: auto;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 0.24rem;
        font-weight: 400;
        line-height: 0.24rem;
        p {
          width: 3.24rem;
        }
        .title{
          color: #C98E49;
        }
        .content{
          color: #654629;
        }
      }
    }
    .videoPlatform{
      position: relative;
      overflow: hidden;
      .videoTitleBox{
        width: 6.9rem;
        height: 0.9rem;
        margin: auto;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 0.2rem;
        .titleIcon{
          width: auto;
          height: 0.5rem;
          margin-right: 0.13rem;
        }
        .titleText{
          flex: 1;
          font-family:PingFang-SC-Regular;
          font-size: 0.32rem;
          font-weight: 400;
          line-height: 0.32rem;
          color: #191919;
        }
        .titleDesc{
          font-family:PingFang-SC-Regular;
          font-size: 0.24rem;
          font-weight: 400;
          line-height: 0.32rem;
          color: #999999;
        }
      }
      .vodeoList{
        width: 6.9rem;
        margin: auto;
        display: flex;
        flex-wrap: wrap;
        border-bottom: 0.01rem solid rgba(238, 238, 238, 1);
        border-top: 0.01rem solid rgba(238, 238, 238, 1);
        .logoItem{
          width: 1.72rem;
          height: 2.25rem;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          .logoBox{
            width: 0.98rem;
            height: 0.98rem;
            display: flex;
            justify-content: center;
            align-items: center;
            background:rgba(255,255,255,1);
            box-shadow:0px 0.02rem 0.04rem 0px rgba(204,204,204,0.5);
            border-radius:0.1rem;
            img {
              width: 0.68rem;
              height: 0.68rem;
            }
          }
          .logoName{
            width: 100%;
            margin-top: 0.2rem;
            font-family:PingFang-SC-Regular;
            font-size: 0.24rem;
            font-weight: 400;
            line-height: 0.24rem;
            text-align: center;
            color: #191919;
          }
        }
        .logoItemBut{
          width: 1.72rem;
          height: 2.25rem;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          .logoBox{
            width: 0.98rem;
            height: 0.98rem;
            display: flex;
            justify-content: center;
            align-items: center;
            background:rgba(255,255,255,1);
            box-shadow:0px 0.02rem 0.04rem 0px rgba(204,204,204,0.5);
            border-radius:0.1rem;
            img {
              width: 0.68rem;
              height: 0.68rem;
            }
          }
          .logoName{
            width: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: 0.2rem;
            font-family:PingFang-SC-Regular;
            font-size: 0.24rem;
            font-weight: 400;
            line-height: 0.24rem;
            text-align: center;
            color: #191919;
            img {
              width: 0.22rem;
              margin-left: 0.03rem;
            }
          }
        }
      }
    }
    .videoBox{
      position: relative;
      width: 6.9rem;
      margin: auto;
      margin-top: 0.14rem;
      padding-bottom: 0.5rem;
      background: #fff;
      .videoTitle{
        width: 6.9rem;
        height: 0.9rem;
        margin: auto;
        display: flex;
        justify-content: center;
        align-items: center;
        border-bottom: 0.01rem solid rgba(238,238,238,1);
        .titleIcon{
          display: block;
          width: 0.07rem;
          height: 0.3rem;
          margin-right: 0.22rem;
          border-radius: 0.04rem;
          background: linear-gradient(-9deg,rgba(250,152,144,1),rgba(254,116,129,1),rgba(254,81,105,1));
          box-shadow:0px 0.04rem 0.1rem 0px rgba(254,121,132,0.5);
        }
        .title{
          flex: 1;
          font-family:PingFang-SC-Regular;
          font-size: 0.32rem;
          font-weight: 400;
          line-height: 0.32rem;
          color: #191919;
        }
        .moreBox{
          display: flex;
          justify-content: center;
          align-items: center;
          .moreIcon{
            width: 0.24rem;
            margin-left: 0.11rem;
          }
          .moreText{
            font-family:PingFang-SC-Regular;
            font-size: 0.24rem;
            font-weight: 400;
            line-height: 0.32rem;
            color: #999999;
          }
        }
      }
      .videoItemBox{
        width: 6.9rem;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        .videoItem{
          position: relative;
          overflow: hidden;
          width: 2.14rem;
          margin-top: 0.4rem;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          .videoImgBox{
            position: relative;
            overflow: hidden;
            float: left;
            width: 2.14rem;
            height: 3.1rem;
            border-radius: 0.1rem;
            .videoImg{
              width:0;
              height:100%;
              padding-left: 100%;
              overflow:hidden;
              background-position: center center;
              background-repeat: no-repeat;
              -webkit-background-size:cover;
              -moz-background-size:cover;
              background-size:cover;
            }
          }
          .videoName{
            width: 80%;
            height: 0.3rem;
            margin-top: 0.2rem;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            font-family:PingFang-SC-Regular;
            font-size: 0.28rem;
            font-weight: 400;
            color: #191919;
            line-height: 0.3rem;
            text-align: center;
          }
        }
      }
    }
  }
 #imgs{
    width: 100% !important;
  }
</style>
