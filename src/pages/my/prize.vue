<template>
  <div class="prize"> 
    <div class="banner">
      <img :src="banner" alt="">
    </div>   
    
    <div class="container">
      <div class="userinfo_box ">
        <div class="userpic lt">
          <img :src="'http://m.shuqian.shop' + info.avatar" alt="">
        </div>
        <div class="userinfo lt">
          <!-- <p class="user_id">{{info.nickname}}</p> -->
          <p class="integral">拥有购物通积分：<span class="integral_num">{{info.credit1}}</span></p>
          <p class="prizenums">剩余抽奖次数：<span class="prizenums">{{info.count}}</span></p>
        </div>
        <div class="record_btn rt" @click="jumpTo('/myrecord')">记录</div>
      </div>
      <div id="gift-box">
        <ul class="prize_box">
          <li :class="[index==0?'active':'']" data-value="二等奖"><p class='numbers'>{{prizeInfo[0].credit}}</p> <div class="names">积分</div></li> 
          <li :class="[index==1?'active':'']" data-value="二等奖"><p class='numbers'>{{prizeInfo[1].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==2?'active':'']" data-value="三等奖"><p class='numbers'>{{prizeInfo[2].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==3?'active':'']" data-value="四等奖"><p class='numbers'>{{prizeInfo[3].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==11?'active':'']" data-value="五等奖"><p class='numbers'>{{prizeInfo[11].credit}}</p> <div class="names">积分</div></li>
          <li  class="hyaline"></li>
          <li  class="hyaline"></li>
          <li :class="[index==4?'active':'']" data-value="六等奖"><p class='numbers'>{{prizeInfo[4].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==10?'active':'']" data-value="七等奖"><p class='numbers'>{{prizeInfo[10].credit}}</p> <div class="names">积分</div></li>
          <li  class="hyaline"></li>
          <li  class="hyaline"></li>
          <li :class="[index==5?'active':'']" data-value="八等奖"><p class='numbers'>{{prizeInfo[5].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==9?'active':'']" data-value="九等奖"><p class='numbers'>{{prizeInfo[9].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==8?'active':'']" data-value="10等奖"><p class='numbers'>{{prizeInfo[8].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==7?'active':'']" data-value="11等奖"><p class='numbers'>{{prizeInfo[7].credit}}</p> <div class="names">积分</div></li>
          <li :class="[index==6?'active':'']" data-value="12等奖"><p class='numbers'>{{prizeInfo[6].credit}}</p> <div class="names">积分</div></li>
        </ul>
        <div v-show="repeat === true" class='state' @click="state">
          <div class="state_btn">
              <img :src="clicks" alt="">
          </div>
          <div class="state_txt">10购物通积分/次</div>
        </div>
        <div v-show="repeat === false" class='state '>
          <div class="state_btn">
              <img :src="clicks" alt="">
          </div>
          <div class="state_txt">10购物通积分/次</div>
        </div>
      </div>
      <div class="fenxiang" @click="jumpTo('/invite')">分享给好友</div>
      <div v-if="recordList.length > 0" class="record_list_box">
        <div class="em">中奖纪录</div>
        <BoBao2 :data="recordList"></BoBao2>
      </div>
    </div>
    <div class="recommend_list">
      <div class="goods_like_box" ref="likes" >
        <img src="../../assets/img/home/goods/likes.png" alt="">
      </div>
      <div class="goodlist_box"  style="display:none">
        <div class="goods_li" v-for="(item,index) of lists" :key="index"  :style="(index%2 ==0) ?'margin-right:.2rem':'margin-right:0rem'"  @click="detail(item)">
          <div class="preview">
            <!-- @click.stop="openVideo(item)" -->
            <img :src="item.thumb"  class="video" >
              <!-- v-if="item.commission_money" -->
            <p class="commission_money" >分享赚：{{item.price1}}</p> 
          </div>
          <div class="con_right">
            <div class="con_quan_box">
              <div class="quan lt">{{item.coupon_money}}元券</div>
              <div class="shouyi rt">预估收益￥{{item.shouyi}}</div>
            </div>
            <div class="goodstitle">
              <p class="brand lt">
                <!-- v-if="Number(item.type) === 11" -->
                <img src="../../assets/img/home/tblogo.png" alt="" >
                <!-- <img src="../assets/img/home/tblogo.png" alt="" v-if="Number(item.type) === 11">
                <img src="../assets/img/home/tmlogo.png" alt="" v-else-if="Number(item.type) === 12">
                <img src="../assets/img/home/jdlogo.png" alt="" v-else-if="Number(item.type) === 21">
                <img src="../assets/img/home/pddlogo.png" alt="" v-else-if="Number(item.type) === 31"> -->
                <!-- {{item.title}} -->
              </p>
              <div class="title lt">{{item.title}}</div>
            </div>
            <!-- <div class="prices">
              <div class="original"><p>:&nbsp;&nbsp;<del>￥123</del></p><i>已售:123</i></div>
            </div> -->
            <!-- <div class="shoptitle">123</div> -->
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
            <!-- <div class="up_gain" v-if="$bizId !== 1 && item.level_commission_money && item.level_commission_money != 0">
              <span v-if="item.commission_money">预估赚:￥{{item.commission_money}}</span>
              <span v-if="item.level_commission_money && item.level_commission_money != 0 && (item.commission_money != item.level_commission_money)">升级赚:￥{{item.level_commission_money}}</span>
            </div> -->
          </div>
      </div>
    </div>
      <goodsListBox v-bind:list = like styles=true  @detail="detail"></goodsListBox>
    </div>
    <div class="prizealert"  v-show="prizealert" :style="{height:heights} " @click.stop="close()">
      <div class="alert">
        <div class="prizetitle">恭喜你中奖了</div>
        <div class="prizecontent">
          <div class="contents">   {{prizeResult}}</div>
           <div class="contents" @click.stop="close()" >关闭</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import GoodsItemEd from '../../components/GoodsListEd';
import BoBao2 from '../../components/BoBao2.vue';
// import BoBao from '../../components/BoBao.vue';
import goodsListBox from '../../components/goodslistbox'
import * as utils from '../../utils'
export default {
  components:{
    GoodsItemEd,
    BoBao2,
    goodsListBox,
  },
  data(){
    return{
    pic:'http://img.alicdn.com/imgextra/i4/2421981497/O1CN01yNBDJK1MvfPtffSq9_!!2421981497.jpg_310x310.jpg',
    speeds:100,
    index: -1,// 当前转动到哪个位置，起点位置
    count: 12,//总共有多少位置
    timer: "",//每次转动定时器
    speed: 100,//初始转动速度
    times: 0,// 转动次数
    cycle: 3,//转动基数：至少需要转动多少次再进入抽奖环节
    repeat: true,
    prize: "",//中奖位置\
    info:"",
    like: '',
    banner:require("../../assets/img/my/banner.png"),
    clicks:require("../../assets/img/my/clicks.png"),
    clickbg:require("../../assets/img/my/clickbg.png"),
    childsbg:require("../../assets/img/my/childsbg.png"),
    childbg:require("../../assets/img/my/childbg.png"),
    lists:"",
    recordList:"",
    prizeInfo:"",
    heights:"",
    prizealert:false,
    prizeResult:"",
  }
},
methods:{
// 开始抽奖
  state(){
    this.getDrawInfo()
    // this.startRoll()
  },
 startRoll :function(){
    console.log("转动次数")
    this.times+=1;//转动次数
    this.oneRoll();// 转动过程调用的每一次转动方法，这里是第一次调用初始化
    this.usePrize();
  },
// 转动的方法
  oneRoll:function(){
    let index = this.index //当前转动到的位置
    this.repeat=false;
    const count = 12 //总共的位置
    index += 1
    if(index >count - 1){
    index = 0
    }
    this.index = index
  },
  //判断是否转到中奖位置，如果是，停止，如果不是，继续转动
    usePrize:function(){
    // 判断是否达到转动次数要求且转到的位置是中奖位置
      // console.log(this.times, this.cycle + 10,this.prize,this.index)
        if(this.times > this.cycle * 10 && this.prize === this.index) {
        //清除定时器，转动停止
        clearTimeout(this.timer) 
        this.times = 0    
        this.speed = this.speeds;
        this.repeat = true;
        // 中奖结果
          console.log(this.index,"中奖结果出来了")
          this.setPrizeSubmit(this.index)
          this.getinfo()
        }else {
        //否则继续转动
        if(this.times<this.cycle){
        this.speed-=3
        }
      this.timer = setTimeout(this.startRoll, this.speed)
      }
    },
    // 获取抽奖信息
      getprizeInfo(){
        this.$http.post('/amoy/lucky/index', {
        }, false, true).then((res) => {
          if (res.code === 0) {
            // balance
            this.prizeInfo = res.data
          }
        })
      },
      // 中奖的接口请求
      getDrawInfo(){
        // 请求次数
        console.log("请求次数")
        this.$http.post('/amoy/lucky/random', {
        }, false, true).then((res) => {
          if (res.code === 0) {
            console.log("请求成功")
            this.prize = res.data.id
            // this.times+=1;//转动次数
            // this.oneRoll();// 转动过程调用的每一次转动方法，这里是第一次调用初始化
            // this.usePrize();
             this.startRoll()
          }else{
            this.$vux.toast.text(res.msg)
            //  this.$vux.toast.text('请输入正确的手机号')
          }
        })
      },
      // 中奖提交
      setPrizeSubmit(inx){
        this.$http.post('/amoy/lucky/prize', {
            credit:this.prizeInfo[inx].credit
          }, false, true).then((res) => {
            if (res.code === 0) {
              // 成功弹窗
              this.prizeResult = this.prizeInfo[inx].credit + '积分'
              this.prizealert=true;
            }else{
               this.$vux.toast.text(res.msg)
            }
          })
      },
      // 获取个人信息
      getinfo(){
        // amoy/lucky/info
        this.$http.post('/amoy/lucky/info', {
          }, false, true).then((res) => {
          if (res.code === 0) {
            this.info = res.data;
            this.recordList = res.data.record
          }else{
            this.$vux.toast.text(res.msg)
          }
        })
      },
      // detail (item) {
      //   this.$emit('detail', item)
      // },
      detail (item) {
        console.log(item,"item")
      var hoder = {};
      var arrs=[];
      arrs.push(item)
      // let arr = utils.storage.get('supergoods')
      // arr.splice((Number(this.$route.query.i) + 1), 1, item)
      utils.storage.set('supergoods', arrs)
      this.$router.push('/supergoods?type=' + item.type + '&id=' + item.origin_id + '&i=' + 0)
    },
    // 获取商品列表 amoy/shop/guess-like
      getlike(){
        this.$http.post('/amoy/shop/guess-like', {
          }, false, true).then((res) => {
          if (res.code === 0) {
          // for (let i = 0; i < res.data.length; i++) {
          //     res.data[i].zk_final_price = res.data[i].origin_price
          //   }
            this.like = res.data
            // this.lists = res.data;
            // console.log(this.lists)
          }else{
            this.$vux.toast.text(res.msg)
          }
        })
      },
      close(){
        this.prizealert = false;
      }
  },
  mounted:function(){
    this.getprizeInfo();
    this.getlike();
    // this.getMyRecord();
    this.getinfo();
    this.heights = document.body.clientHeight + "px";
  }
}
</script>
<style scoped lang='less'>
*{margin:0;padding:0;}
.prize{
  background: #f7f7f7;
  position: relative;
  .banner{
    height: 2.8rem;
    img{
      width: 100%;
    }
  }
  .container{

  }
  .container{
    background: #ffdecf;
    padding: .2rem;
    .userinfo_box{
      background: white;
      height: 1.95rem;
      border-radius: .1rem;
      padding: .25rem .2rem;
      box-sizing: border-box;
      // display: flex;
      // flex-wrap: wrap;
      .userpic{
        width: 1.2rem;
        height: 1.2rem;
        // background: #00aaee;
        border-radius: 50%;
      }
      .userinfo{
        margin-left: .2rem;
        .user_id{
          font-size: .3rem;
          color: black;
          font-weight: 800;
        }
        .integral{
          font-size: .3rem;
          color: #676767;
          .integral_num{
            color: #ff5848;
          }
        }
        .prizenums{
          font-size: .3rem;
          color: #676767;
          .prizenums{
            color: #ff5848;
          }
        }
      }
      .record_btn{
        width: 1rem;
        height: .44rem;
        line-height: .44rem;
        text-align: center;
        border:1px solid #ff6354;
        color: #ff6354;
        border-radius: .5rem;
       }
     }
     #gift-box{
       width: 6.7rem;
       height: 6.7rem;
        background: linear-gradient(#fe8a59, #ff7070); 
        border-radius: .15rem;
        margin:.3rem auto;
        position: relative;
        padding: .01rem;
        box-sizing: border-box;
        .prize_box{
          margin: .3rem auto;
          width:6.1rem;
          height:6.1rem;
          list-style:none; 
          display: flex;
          flex-wrap:wrap;
          box-sizing: border-box;
          padding-left: 0.05rem;
        }
     }
     .fenxiang{
      width: 6.25rem;
      height: .9rem;
      background:  linear-gradient(to right,#f97b65, #f63f37);
      margin: 0.5rem auto 0.5rem;
      text-align: center;
      line-height: .9rem; 
      font-size: .3rem;
      color: white;
      border-radius: .5rem;
    }
    .record_list_box{
      width: 100%;
      min-height: 4.5rem;
      background:white;
      border-radius: .15rem;
      margin-bottom: .3rem;
      .em{
        height: .9rem;
        line-height: .9rem;
        color: black;
        font-size: .3rem;
        font-weight: 800;
        text-align: center;

      }
      .record_list{
        height: .9rem;
        line-height: .9rem;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        border-top: 1px solid #eeeeee;
        .userids{
          width: 2rem;
          margin-left: .2rem;
          font-size: .27rem;
          color: black;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        .integral{
          width: 1.5rem;
          text-align: center;
          font-size: .27rem;
          color: black; 
        }
        .reacrdtime{
          width: 2.5rem;
          margin-right: .2rem;
          font-size: .2rem;
          color: #9a9a9a;
        }
      }
    }
  }
  .recommend_list{
    .goods_like_box{
      margin-top: 0.1rem;
      display: flex;
      align-items: center;
      justify-content: center;
      height: .9rem;
      background: #fff;
      img{
        width: 4rem;
      }
    }
    .emtitle{
      height: 0.9rem;
      line-height: .9rem;
      text-align: center;
      background: white;
      margin-top: .2rem;
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
  }
}

.red{
  background: red;
}
.state{
  width: 2.9rem;
  height: 2.9rem;
  text-align: center;
  line-height: 2.8rem;
  position: absolute;
  top: 1.9rem;
  left: 1.9rem;
  background: url("../../assets/img/my/clickbg.png") no-repeat ;
  background-size:100% 100%; 
  border-radius: .1rem;
  overflow: hidden;
  .state_btn{
    position: absolute;
    width: 2.34rem;
    height: 1.67rem;
    top: 50%;
    left: 50%;
    margin-top: -1.1rem;
    margin-left: -1.17rem;
    border-radius: 0.15rem;
    overflow: hidden;
  }
  .state_txt{
    position: absolute;
    bottom:.3rem;
    left: 0;
    height: .4rem;
    width: 100%;
    text-align: center;
    font-size: .3rem;
    line-height: .4rem;
  }
}
.hyaline{
  opacity: 0;
}
ul li,ul a{
  width:1.4rem;
  height:1.4rem;
  /* border:1px solid #565656; */
  text-align:center;
  line-height:1rem;
  background:url("../../assets/img/my/childbg.png") no-repeat;
  background-size: 100%;
  margin: 0.05rem;
  border-radius: .1rem;
  }
ul a:hover{
  cursor:pointer;
  color:orange;
  font-size:18px;}
ul .active{
  background:url("../../assets/img/my/childsbg.png") no-repeat;
  background-size: 100%;
  color:#fff;}
.cur {
  background-color: #ccc;
  }
  .numbers{
    width: 1rem;
    height: .6rem;
    text-align: center;
    font-size: .4rem;
    color: #da2a03;
    line-height: .6rem;
    font-weight: 800;
    margin: .2rem auto 0;
    overflow: hidden;
  }
  .names{
    color: #7b2816;
    height: .4rem;
    line-height: .4rem;
    text-align: center;
    overflow: hidden;
  }
.prizealert{
    width: 100%;
    background: rgba(76, 76, 76, .5);
    position: fixed;
    top: 0;
    left: 0;
    z-index: 3000;
    .alert{
      width: 4.8rem;
      height: 4.8rem;
      background: #fff;
      border-radius: .2rem;
      position: absolute;
      top: 60%;
      left: 50%;
      margin-left: -2.4rem;
      margin-top: -4rem;
      padding: .2rem;
      box-sizing: border-box;
      .prizetitle{
        margin-top: .3rem;
        width: 100%;
        text-align: center;
        color: #da2a03;
        font-size: .35rem;
      }
      .prizecontent{
        margin-top: .1rem;
        text-align: center;
        font-size: .4rem;
        color: #da2a03;
        height: 3rem;
        width: 100%;
        .contents{
           height: 3rem;
           width: 100%;
        }
        .contents{
          text-align: center;
          color: #000;
          font-size: .25rem;
        }
      } 
    }
}
  .lt{float: left;}
  .rt{float: right;}
</style>


