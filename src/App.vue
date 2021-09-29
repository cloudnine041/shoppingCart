// 掌握了父子组件传值用自定义属性和自定义事件，
// 兄弟组件或者有深层嵌套关系组件之间传值用eventBus，
// 以及数组的高级方式，包括some,filter,every,reduce,forEach

<template>
  <div class="app-container">
    <!-- 规范：先写属性：，再写事件@ -->
    <Header title="购物车案例"></Header>

    <Goods v-for="item in list"
    :key="item.id"
    :id="item.id"
    :title="item.goods_name"
    :pic="item.goods_img"
    :price="item.goods_price"
    :state="item.goods_state"
    :count="item.goods_count"
    @state-change="getNewState"></Goods>

    <Footer :isfull="fullState"
    :amount="amt"
    :allNum="allCount"
    @full-change="getFullState"></Footer>
  </div>
</template>

<script>
import axios from 'axios'
//导入组件首字母大写
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
import eventBus from '@/components/eventBus.js'

export default {
  data() {
    return {
      //购物车列表
      list: []
    }
  },
  components: {
    Header,
    Goods,
    Footer
  },
  methods: {
    //封装axios
    async initCartList() {
      const { data:res } = await axios.get('https://www.escook.cn/api/cart')
      if(res.status === 200 )
        this.list = res.list
    },
    getNewState(val) {
      this.list.some(item => {
        if(item.id === val.id){
          item.goods_state = val.value
          return true
        }
      })
    },
    getFullState(val) {
      this.list.forEach(item => item.goods_state = val)
    }
  },
  computed: {
    //计算属性的特征就是可以实现计算数学随数据源动态改变
    fullState() {
      return this.list.every(item => item.goods_state)
    },
    amt() {
      return this.list.filter(item => item.goods_state).reduce((total,item) => total += item.goods_price * item.goods_count,0)
    },
    allCount() {
      return this.list.filter(item => item.goods_state).reduce((total,item) => total += item.goods_count,0)
    }
  },
  created() {
    this.initCartList()
    eventBus.$on('share',val => {
      this.list.some(item => {
        if(item.id === val.id){
          item.goods_count = val.value
          return true
        }
      })
    })
    eventBus.$on('subCount',val => {
      this.list.some(item => {
        if(item.id === val.id){
          item.goods_count = val.value
          return true
        }
      })
    })
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
