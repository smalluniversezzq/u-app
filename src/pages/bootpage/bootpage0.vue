<template>
  <div class="bootpage0"  > 
   <router-view class="view"></router-view>
  </div>
</template>
<script>
export default {
  data(){
    return{
    }
  },
  methods:{
     to(){
      this.$router.push({
        name: 'page2'
        })
     }
  },
  mounted:function(){
    window.canRightSlipBack = false
  },
  destroyed:function(){
    window.canRightSlipBack = true
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

}
</script>

