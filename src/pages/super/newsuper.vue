<template>
  <div id="sort">
    <!-- <div class="header" :style="{paddingTop: statusH + 'rem',color: $store.state.global.search.color,background: 'linear-gradient('+$store.state.global.search.bgDeg+'deg,' +$store.state.global.search.bgColor+','+$store.state.global.search.bgSecondColor+')'}">
      <div class="searchBox" @click="jumpTo('/search?type=0')"  :style="{background: $store.state.global.search.searchBgColor}">
        <img :src="$store.state.global.search.searchIcon" alt="">
        <p :style="{color:$store.state.global.search.color}">{{$store.state.global.search.searchPlaceholder}}</p>
      </div>
    </div> -->
    <!-- -->
    <div class="sort_con" v-if="data" :style="{top: (0 ) + 'rem'}"  >
      <div class="sort_left" ref="left">
        <ul>
          <template v-for="(item, index) in datas">
            <li  :key="index" v-if="colorIndex === index" class="red"  style="background:#fff;"  @click="change(item, index)">
                <!-- <div :style="{color: $store.state.global.theme.mainColor}" > {{item.main_name}}</div> -->
                <div style="color:#ff5846" > {{item.title}}</div>
              </li>
            <li  :key="index" v-else @click="change(item,index)">
              <div>{{item.title}}</div>
            </li>
          </template>
        </ul>
        <!-- <span :style="{'top': offTop,background: $store.state.global.theme.mainColor}"></span> -->
        <span :style="{'top': offTop,background: colors}"></span>
      </div>
      <div class="sort_right" ref="right" >
        <div ref="right" class="banner_box"  v-if ="datas[colorIndex].cid === 0" >
          {{colorIndex}}
          <div class="banner">
            <div class="banner_pic ">
              <img :src="bannerbg" alt="">
            </div>
            <div class="searchBox" @click="jumpTo('/search?type=0')"  :style="{background: $store.state.global.search.searchBgColor}">
              <img :src="jing" alt="">
              <p style="color:#999">请输入宝贝名称或标题</p>
            </div>
          </div>
          <div class="bg">
            <div class="sort_right_for" v-for="(item, index) in datas[colorIndex].content.super" :ref="'right'+index" :key="index" :class="[colorIndex === index ? 'redCopy' : '']">
              <h2 class="supertitle">
                <!-- <span></span> -->
                <p>{{item.title}}</p>
                <!-- <span></span> -->
              </h2>
              <ul class="super_list_box">
                <!-- 跳转淘宝  url-->
                <!-- <li class="super_list" v-for="(t, i) in item.child" :key="i" @click="open(t.url)"> -->
                <li class="super_list" v-for="(t, i) in item.child" :key="i" @click="open(t.url)"  :style="{borderBottom:(i > (item.child.length -5)) ? '1px solid #eeeeee' : 'none'}">
                  <div class="pics">
                    <img v-lazy="t.img" alt="" :key="t.img">
                  </div>
                  <p class="super_title">{{t.name}}</p>
                  <p class="fanyong">{{t.describe}}</p>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <!-- 显示2 -->
        <div ref="right" class="banner_boxs"  v-if ="datas[colorIndex].cid !== 0" >
          <div class="banner">
            <div class="searchBox" @click="jumpTo('/search?type=0')"  :style="{background: $store.state.global.search.searchBgColor}">
                 <img :src="jing" alt="">
              <p style="color:#999">请输入宝贝名称或标题</p>
            </div>
            <div class="banner_pic" v-for="(itempic,indexpic) in datas[colorIndex].content.headimg" :key="indexpic" @click="open(itempic.url)" >
              <img :src="itempic.pic" alt="">
            </div>
          </div>
          <div class="bg" style="display:block">
            <!-- v-for="(item, index) in datas[colorIndex].content.super" :ref="'right'+index" :key="index" :class="[colorIndex === index ? 'redCopy' : '']" -->
            <div class="sort_right_for" v-if="datas[colorIndex].content.super.child.length > 0" >
              <!-- {{item}} -->
              <h2 class="supertitle">
                 <!-- <span></span> -->
                <p>{{datas[colorIndex].title}}</p>
                <!-- <span></span> -->
              </h2>
              <ul class="super_list_box" >
              <!-- //跳转商品 -->
               <li  class="super_list" v-for="(ts, ins) in datas[colorIndex].content.super.child"  :key="ins"  @click="open(ts.url)" :style="{borderBottom:(ins > (datas[colorIndex].content.super.child.length -5)) ? '1px solid #eeeeee' : 'none'}" >
                  <div class="pics">
                    <img id="imgs" v-lazy="ts.img" alt="" style='borderRadius:1px;' :key="ts.img">
                  </div>
                  <p class="super_title">{{ts.name}}</p>
                  <p class="fanyong">{{ts.describe}}</p>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'newsuper',
  data () {
    return {
      data: '',
      datas:"",
      colorIndex: 0,
      offTop: '.4rem',
      width: '',
      flag: true,
      right: false,
      statusH: '',
      colors:"#ff5846",
      bannerbg:require("../../assets/img/super/bannerbg1.png"),
      jing:require("../../assets/img/super/1.png"),
      pic:[
        {bandel:require("../../assets/img/super/bandel1.png"),},
        {bandel:require("../../assets/img/super/bandel2.png"),}
      ],
    }
  },
  created () {
    this.width = document.documentElement.clientWidth || document.body.clientWidth
    this.size = this.width / 750 * 100
    let size = (document.documentElement.clientWidth / 7.5)
    this.statusH = (api.safeArea.top / size)
  },
  methods: {
    getInfo () {
      this.$http.post('/amoy/hao/super-classify', {}, true).then(res => {
        this.data = res.data
        console.log(this.data,"data")
      })
    },
    getnewInfo(){
       this.$http.post('/amoy/shop/super', {}, true).then(res => {
        this.datas = res.data;
        console.log(res.data,"datanew**************")
      })
    },
    change (item, index) {
      this.colorIndex = index
      this.$nextTick(() => {
        let red = document.getElementsByClassName('red')
        this.offTop = red[0].offsetTop / this.size + 0.4 + 'rem'
        this.$refs.right.scrollTop = 0
      })
    },
    open (url) {
      console.log("跳转前")
      api.openWin({
        name: 'aliWin',
        url: '../html/main.html',
        reload: true,
        pageParam: {
          url: url,
          back: 'newsuper'
        },
        animation: {
          type: 'push',
          subType: 'from_right',
          duration: 300
        },
        bounces: false,
        slidBackEnabled: false,
        allowEdit: true
      })
    }
  },
  updated () {
  },
  mounted () {
    this.getInfo();
    this.getnewInfo();
  }
}
</script>

<style scoped lang="less">
#sort{
  background: #fff;
  position: relative;
  .searchBox{
    img{
      width: .3rem;
      height: .3rem;
    }
    p{
      margin-top: 0.02rem;
    }
  }
  .sort_con{
    position: absolute;
    left: 0;
    width: 100%;
    bottom: 0;
  }
}
.jingxuan{
  font-size: 0.3rem;
  padding: 0.35rem 0;
  text-align: center;
  color: #333;
  line-height: 0.3rem;
}
#sort .sort_con .sort_left span{
  width: 0.1rem;
  height: .4rem;
  border-radius:0 !important;
}
#sort .sort_con .sort_left ul li{
    font-size: 0.3rem;
    text-align: center;
    color: #333;
    line-height: 0.3rem;
    margin: 0;
    padding: 0;
    background: #f6f6f6;
}
#sort .sort_con .sort_left ul li div{
    font-size: 0.3rem;
    padding: 0.35rem 0;
    text-align: center;
    color: #333;
    line-height: 0.3rem;
}
.sort_left{
  border: none  !important;
}
.sort_right{
  .banner_box{
    .banner{
      margin: 0 auto;
      width: 5.44rem;
      height: 3rem;
      border-radius: .15rem;
      position: relative;
      .banner_pic{
        border-radius: .15rem;
      }
      .searchBox{
        position: absolute;
        top: 2rem;
        left: .4rem;
        -webkit-box-flex: 1;
        -ms-flex: 1;
        flex: 1;
        height: 0.6rem;
        border-radius: 0.3rem;
        margin-top: .2rem;
        // background: rgba(246, 246, 246, .5) !important;
        display: flex;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        width: 4.5rem;
        height: .57rem;
        background: #fff !important;
        margin: 0 auto !important;
        img{
          width: 0.4rem;
          height: 0.4rem;
          margin-left: 0.3rem;
        }
        p{
        margin-top: 0.02rem;
        margin-left: 0.2rem;
        font-size: 0.26rem;
        color: #000 !important;
        text-align: center;
        }
      }
    }
    // 内容
    .bg{
      background: #f6f6f6;
      .sort_right_for{
        background: #fff;
        margin-bottom: .2rem;
        .supertitle{
          padding: 0.3rem 0 !important;
        }
        .super_list_box{
          .super_list{
            border-top: 1px solid #eeeeee;
            border-right: 1px solid #eeeeee;
            width: 25% !important;
            box-sizing: border-box;
            .pics{
              margin-top: .15rem;

            }
            .super_title{
              padding-bottom: 0 !important;
              color: #333 !important;
              font-size: .24rem;
            
            }
            .fanyong{
              padding-bottom: 0.2rem !important;
              color: #666  !important;
              font-size: .18rem !important;
            }
          }
        }
      }
    }
  }
}
// 样式二
  .banner_boxs{

    .banner{
      margin: 0 auto;
      width: 5.44rem;
      min-height: 3rem;
      border-radius: .15rem;
      position: relative;
      margin-top: .2rem;
      .searchBox{
        -webkit-box-flex: 1;
        -ms-flex: 1;
        flex: 1;
        border-radius: 0.3rem;
        background: #eeeeee !important;
        display: flex;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        width: 100%;
        height: .57rem;
        margin: 0 auto !important;
        img{
          width: 0.4rem;
          height: 0.4rem;
          margin-left: 0.3rem;
        }
        p{
        margin-top: 0.02rem;
        margin-left: 0.2rem;
        font-size: 0.26rem;
        color: #000 !important;
        text-align: center;
        }
      }
      .banner_pic{
        margin-top: .2rem;
         border-radius: .15rem;
         overflow: hidden;
      }
    }
    // 内容
    .bg{
      background: #f6f6f6;
      .sort_right_for{
        background: #fff;
        margin-bottom: .2rem;
        .supertitle{
          padding: 0.3rem 0 !important;
        }
        .super_list_box{
          .super_list{
            border-top: 1px solid #eeeeee;
            border-right: 1px solid #eeeeee;
            width: 25% !important;
            box-sizing: border-box;
            // height: 1.95rem;
            overflow: hidden;
            .pics{
              margin-top: .15rem;
              border-radius: none;
              img{
                border-radius: none !important;
              }
            }
            .super_title{
              line-height: 0.24rem;
              padding-bottom: 0 !important;
              height: .35rem;
              overflow: hidden;
              color: #333 !important;
              font-size: .24rem;

            }
            .fanyong{
              line-height: 0.24rem;
              padding-bottom: 0.02rem !important;
              padding-top: 0 !important;
              height: .45rem;
              overflow: hidden;
              color: #666  !important;
              font-size: .18rem !important;
            }
          }
        }
      }
    }
  }
#app .vux-header{
  border-bottom: none !important;

}
#sort .sort_con .sort_right .sort_right_for ul li img{
  // border: 1px solid #eeeeee; 
  // box-sizing: border-box;   
}
</style>
