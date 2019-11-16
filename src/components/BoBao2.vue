<template>
  <div class="newdanmu">
    <div id="barrage">
      <div class="bobao" :style="{top:'-' + tops + 'rem'}">
        <div class="record_list" v-for="(item,index) in data" :key='index'>
          <div class="userids">{{item.remark}}</div>
          <div class="integral">{{item.price}}积分</div>
          <div class="reacrdtime">{{item.created_at}}</div>
        </div>
        </div>
    </div>
  </div>
</template>
<script>
import * as utils from "../utils";
// import { clearInterval } from 'timers';
export default {
  components: {
  },
  // props:['data'],
  data() {
    return {
      upbarrage: "",
      data:"",
      datas:[
        // {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"10",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //     {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"11",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //     {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"12",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //     {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"13",
        //   created_at:"2019-05-06 10:48:08"
        // },
        // {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"13",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //         {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"10",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //     {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"11",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //     {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"12",
        //   created_at:"2019-05-06 10:48:08"
        // },
        //     {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"13",
        //   created_at:"2019-05-06 10:48:08"
        // },
        // {
        //   remark:"每日抽奖获得积分9,已存入积分银行",
        //   price:"13",
        //   created_at:"2019-05-06 10:48:08"
        // },
        
      ],
      tops:0,
      totop:.9,
      timer: '',
      value:0,
      datalist:"",
    };
  },
  created: function() {},
  mounted: function() {
     this.timer = setInterval(this.get, 5000);
     this.getinfo()
    // console.log(typeof(this.nums))
  },
  methods: {
    get(){
     this.value ++;
     this.data.length;
      if(this.data.length == this.value){
        this.tops = 0;
        this.value = 0;
      }else{
      this.tops = this.tops + this.totop;
      }
    },
    getinfo(){
      this.$http.post('/amoy/lucky/info', {
        }, false, true).then((res) => {
        if (res.code === 0) {
          this.data = res.data.record
          console.log(this.data)
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    },
  },
  destroyed(){
     clearInterval(this.timer);
  }
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
  overflow: hidden;
  height: 3.6rem;
  position: relative;
  .bobao{
    position: absolute;
    top: 0;
  }
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
  /* .swiper-slide{
    text-align: left;
    font-size:.3rem;
    overflow: hidden;
  } */
  .record_list{
  box-sizing: border-box;
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



