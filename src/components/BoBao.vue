<template>
  <div class="newdanmu">
    <div  id="barrage">
      <swiper  id="Headline" :options="swiperOptions" ref="swiperBox">
        <swiper-slide v-for="(item,index) in data" :key="index">
          <div class="bobao">
            <div class="record_list">
              <div class="userids">{{item.remark}}</div>
              <div class="integral">{{item.price}}积分</div>
              <div class="reacrdtime">{{item.created_at}}</div>
            </div>
        </div>
        </swiper-slide>
      </swiper>
    </div>
  </div>
</template>
<script>
import * as utils from "../utils";
import { swiper, swiperSlide } from "vue-awesome-swiper";
import "swiper/dist/css/swiper.css";
import { setTimeout, setInterval } from 'timers';
export default {
  components: {
    swiper,
    swiperSlide
  },
  props:['data'],
  data() {
    return {
      upbarrage: "",
      swiperOptions: {
        direction : 'vertical',
        grabCursor: true,
        loop: false,
        slidesPerView: 1,
        autoplay: {
          delay: 4000,
          disableOnInteraction: false
        },
      }
    };
  },

  created: function() {},
  mounted: function() {
      this.getBarrage() 
      var i=1
      setInterval(function(){
          this.mySwiper.autoplay.start();
      },5000)
  },
  methods: {
    getBarrage() {
      this.$http.post("/amoy/user/new-orders", {}, false, true).then(res => {
        if (res.code === "0") {
          this.upbarrage = res.data.list;
        }
      });
    },

  },
};
</script>
<style  lang="less" scoped>
.newdanmu{
  height:.8rem;
  background: #fff;
  padding: 0 .2rem;
  margin-top:-0.02rem;
}
#Headline {
  border-radius: 0.4rem;
  line-height: 0.8rem;
  height: 0.8rem;
  text-align: right;
  padding-right: .1rem;
}
#barrage {
  position: relative;
  overflow: hidden;
}
.barrage-box {
  width: 100%;
}
.lv {
  color: red;
}
.tic {
  color: red;
}

</style>
<style scoped>
  .swiper-slide{
    text-align: left;
    font-size:.3rem;
    overflow: hidden;
  }
  .record_list{
  height: .9rem;
  line-height: .9rem;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  border-bottom: 1px solid #eeeeee;
  }
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

  
</style>



