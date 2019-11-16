<template>
  <div class="home_list" v-if="list">
    <div class="header_count">
      <ul>
        <li v-for="(item,index) in list"  @click="handDetail(item.id)" :key="index">
          <img v-lazy="item.thumb" alt="">
          <div class="shop_name">
            <table></table>
            <p>{{item.goodsName}}</p>
            <!-- <div class="market-price"> -->
            <div class="market-price">
              <!-- orders0 -->
              <div v-if=" item.commission != 0 " class="commiss">佣金<span>{{item.commission}}</span></div>
            </div>
            <div class="price">
              <!-- order0 -->
              <div class="price_main">积分<span>{{item.credit}}</span></div>
              <span>已售：{{item.sales}}</span>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'list',
  props: ['list'],
  methods: {
    handDetail (id) {
      this.$router.push({
        name: 'mallHomeDetail',
        query: {
          title: id
        }
      })
    }
  }
}
</script>

<style scoped>
.commiss{
    color: #fc3357;
    font-size: 0.3rem;
}

</style>
