<template>
  <div class="bootpage0" > 
    <div class="pages">
      <img :src="pic[3]" alt="">
    </div>
    <div class="close"  @click="jumpTo('/home')">
    </div>
    <div class="next"  @click="to('page5')">
    </div>
  </div>
</template>
<script>
import * as utils from '../../utils'
export default {
  data(){
    return{
      pic:"",
      heights:"",
    }
  },
  methods:{
     to(val){
        this.$router.push({
          name: val
      })
    }
  },
    computed: {
    h () {
      let size = (document.documentElement.clientWidth / 7.5)
      let height = 0
      if (window.api) {
        height = api.safeArea.top >= 30 ? api.safeArea.top + 5 : api.safeArea.top
      }
      return (height / size)
      return 1
    }
  },
  mounted:function(){
    this.heights = document.body.clientHeight + "px";
     this.pic = utils.storage.get('bootpage')
    window.canRightSlipBack = false

  },
    destroyed:function(){
    window.canRightSlipBack = true
  }
}
</script>
<style scoped lang="less">
  .bootpage0{
    position: relative;
    .pages{
      width: 100%;
       height: 100%;
      overflow: hidden;
    }
    .close{
      width: 100%;
      height: .6rem;
      position: absolute;
      top: .4rem;
      background: rgba(250, 250, 250, 0);
      z-index: 99;
    }
    .next{
      position: absolute;
      width: 100%;
      height: 10rem;
      top: 5rem;
      background: rgba(250, 250, 250, .5);
      z-index: 99;
    }
  }
</style>
