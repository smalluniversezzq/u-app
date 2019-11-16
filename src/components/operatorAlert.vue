<template>
  <div class="rulealert" @click.stop='off()'>
    <div  class="alert"  @click.stop ='nulls()' >
      <div class="tab">
        <div class="tabs ">收益统计解释说明</div>
         <!-- <div class="close"  @click.stop='off()'>X</div> -->
      </div>
      <div class="alert_content" >
        <ul  class="contentone" >
          <li v-if="explain" v-html="explain"></li>
          <!-- <li class='contentone_list' v-for="(item,index) in listalert" :key="index">
            <p class='title'>
              <i class='em '></i> 
              {{item.title}}</p>
            <div class=list_txt>{{item.txt}}</div>
          </li> -->
        </ul>   
        <div class='closes'  @click.stop='off()'>关闭</div>   
      </div>  
    </div>
  </div>
</template>
<script>
export default {
  name:'operatorAlert',
  data(){
    return{
      tablt:true,
      tabrt:false,
       explain:"",
      listalert:[
        {
          title:"预估佣金",
          txt:"指通过平台成功下单，预计会获得的佣金数量。该金额为预估金额，实际以淘宝、拼多多、京东等第三方网站结算金额为准。"
        },
        {
          title:"预估佣金",
          txt:"指通过平台成功下单，预计会获得的佣金数量。该金额为预估金额，实际以淘宝、拼多多、京东等第三方网站结算金额为准。"
        },
             {
          title:"预估佣金",
          txt:"指通过平台成功下单，预计会获得的佣金数量。该金额为预估金额，实际以淘宝、拼多多、京东等第三方网站结算金额为准。"
        },
        {
          title:"预估佣金",
          txt:"指通过平台成功下单，预计会获得的佣金数量。该金额为预估金额，实际以淘宝、拼多多、京东等第三方网站结算金额为准。"
        },     {
          title:"预估佣金",
          txt:"指通过平台成功下单，预计会获得的佣金数量。该金额为预估金额，实际以淘宝、拼多多、京东等第三方网站结算金额为准。"
        },
        {
          title:"预估佣金",
          txt:"指通过平台成功下单，预计会获得的佣金数量。该金额为预估金额，实际以淘宝、拼多多、京东等第三方网站结算金额为准。"
        },  
      ]
    }
  },
  methods:{
    off(){
     this.$store.commit('setshowoperator',false)
    window.canRightSlipBack = true
    },
    nulls(){},
          // 获取alert说明
    getalerts(){
     this.$http.post('/amoy/user/explain', {
        }, false, true).then((res) => {
        if (res.code === 0) {
          this.explain = res.data;
        }else{
          this.$vux.toast.text(res.msg)
        }
      })
    },
  },
  mounted:function(){
    // if(this.$store.state.global.setshowoperator === true){
      this.getalerts()
    // }
  },
  destroyed:function(){
  }
}
</script>
<style lang='less' scoped>
  .rulealert{
    width: 100%;
    height: 100%;
    background: rgba(76, 76, 76, .5);
    position: absolute;
    top: 0;
    left: 0;
    z-index: 3000;
    .alert{
      width: 6rem;
      min-height: 6rem;
      background: #fff;
      border-radius: .2rem;
      position: absolute;
      top: 50%;
      left: 50%;
      margin-left: -3rem;
      margin-top: -5rem;
      position: relative;
      .tab{
        width: 6rem;
        height: .74rem;
        text-align: center;
        line-height: .74rem;
        font-size: .3rem;
        display: flex;
        flex-wrap:wrap;
        border-bottom: .02rem solid #dadada;
      .close{
        line-height: .74rem;
        }
      .tabs{
        width: 100%;
        text-align: center;
        }
      }
      .alert_content{
        padding: .2rem;
        max-height: 10rem;
        overflow: scroll;
        .contentone{
           min-height: 4rem;
          .contentone_list{
            margin-bottom: .3rem;
            .title{
              display: flex;
              flex-wrap:wrap;
              font-size: .28rem;
              color: #000;
              line-height: .3rem;
              .em{
                display: block;
                width: 0.1rem;
                height: 0.3rem;
                background: #f97b65;
                margin-right: .1rem;
              }
            }
            .list_txt{
              font-size: .2rem;
              color: #737373;
              margin-top: .1rem;
            }
          }
        }
        .closes{
          text-align: center;
        }
      }
    }
  }
.lt{float: left;}
.rt{float: right;}
.active{
  color: #d43e43;
  border-bottom: .02rem solid #d43e43;
}
</style>


