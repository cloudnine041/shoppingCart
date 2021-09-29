<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import eventBus from '@/components/eventBus.js'
export default {
  props: {
    num: {
      default: 1,
      type: Number
    },
    //用eventbus把数量传给app.vue
    id: {
      type: Number,
      required: true
    }
  },
  methods: {
    add() {
      const obj = { id: this.id, value: this.num+1 }
      eventBus.$emit('share',obj)
    },
    sub() {
      const obj = { id: this.id, value: this.num-1 }
      eventBus.$emit('subCount',obj)
    }
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
