<template>
    <div class="EditImg" :style="{backgroundColor: data.bgColor}">
      <div v-for="(item, index) of data.items"  v-if="item.link.url === '/operatorcenter'" :key="index" class="oneImg" :style="{flex: item.flex}" @click="opens()"  >
        <img  :src="item.img" alt="">
      </div>
      <div v-for="(item, index) of data.items"  v-if="item.link.url !== '/operatorcenter'" :key="index" class="oneImg" :style="{flex: item.flex}" @click="jumpLink(item.link)" >
        <img  :src="item.img" alt="">
      </div>
    </div>
</template>

<script>
// partnership
export default {
  name: 'EditImg',
  components: {},
  props: ['data','states'],
  data () {
    return {}
  },
  methods: {
    opens(){
      if(this.states === '普通会员'){
       this.$router.push("/profit")
      }else if(this.states === '合伙人'){
        this.$router.push({
          path:"/operatorcenter",
          query:{
            state:"hehuoren"
          }
        })
      }else if(this.states === '运营商'){
        this.$router.push({
          path:"/operatorcenter",
          query:{
            state:"operatorcenter"
          }
        })
      }
    }
  },
  mounred:function(){
  }
}
</script>

<style scoped lang="less">
.EditImg{
  margin-top: -1px;
  overflow: hidden;
  width: 100%;
  display: flex;
  .oneImg{
    overflow: hidden;
    img{
      float: left;
    }
  }
}
</style>
