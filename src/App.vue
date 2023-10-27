<template>
  <div>
    <indicator ref="indicator" :list="list" :value="index" :max-indicator-dots="5" :dot-size="100" />
    <div style="margin-top: 20px">
      <button @click="prev">点我往前滚动</button>
      <button @click="next">点我往后滚动</button>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import Indicator from './Indicator.vue'

export default defineComponent({
  components: { Indicator },

  data() {
    return {
      list: Array(10).fill('url'),
      index: 0
    }
  },

  methods: {
    prev() {
      this.index > 0 && this.index--
    },
    next() {
      this.index < this.list.length - 1 && this.index++
    },
  }
})
</script>
