<template>
  <div class="indicator" :style="{'--dot-size': `${dotSize}px`, maxWidth: `${dotSize * 2 * maxIndicatorDots}px`}">
    <div class="indicator-inner" style="transition: transform .2s" :style="{transform: `translateX(${translateX}px)`}">
      <div
        v-for="(_, i) in list"
        :key="i"
        class="indicator-dot"
        :class="{ active: i === index, placeholder: i <= leftPlaceholderEnd || i >= rightPlaceholderEnd }"
      >
        <div class="indicator-dot-inner" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Indicator',

  props: {
    list: Array,
    value: Number,
    // 最多显示多少指示器圆点
    maxIndicatorDots: { type: Number, default: 5 },
    // 圆点大小，必须是偶数
    dotSize: { type: Number, default: 8 }
  },

  data() {
    return {
      index: 0,
      translateX: 0,
      leftPlaceholderEnd: -1,
      rightPlaceholderEnd: Number.MAX_SAFE_INTEGER
    }
  },

  watch: {
    value(nextIndex) {
      const currentIndex = this.index
      if (nextIndex === currentIndex) return
      nextIndex > currentIndex ? this.goBehind() : this.goForward()
      this.index = nextIndex
    }
  },

  methods: {
    // 初始化指示器位置
    init() {
      this.index = 0
      this.translateX = 0
      this.leftPlaceholderEnd = -1
      this.rightPlaceholderEnd = this.list.length > this.maxIndicatorDots ? this.maxIndicatorDots - 1 : Number.MAX_SAFE_INTEGER
    },
    // 向前切换图片
    goForward() {
      const total = this.list.length
      const max = this.maxIndicatorDots
      let originIndex = this.index
      const nextIndex = originIndex - 1
      const leftPlaceholderEnd = this.leftPlaceholderEnd

      if (nextIndex !== leftPlaceholderEnd) return

      // 指示器右移，显示左侧更多的圆点
      this.translateX += this.dotSize * 2

      /* 判断左侧占位符的位置 */
      this.leftPlaceholderEnd = nextIndex + 1 > 2 ? nextIndex - 1 : -1
      /* 判断右侧占位符的位置 */
      this.rightPlaceholderEnd = total - nextIndex >= max ? nextIndex + max - 2 : Number.MAX_SAFE_INTEGER
    },
    // 向后切换图片
    goBehind() {
      const total = this.list.length
      const max = this.maxIndicatorDots
      let originIndex = this.index
      const nextIndex = originIndex + 1
      const rightPlaceholderEnd = this.rightPlaceholderEnd

      if (nextIndex !== rightPlaceholderEnd) return

      // 指示器左移，显示右侧更多的圆点
      this.translateX -= this.dotSize * 2

      /* 判断左侧占位符的位置 */
      this.leftPlaceholderEnd = (nextIndex + 2) > max ? (nextIndex + 2) - max : -1
      /* 判断右侧占位符的位置 */
      this.rightPlaceholderEnd = total - (nextIndex + 1) < 2 ? Number.MAX_SAFE_INTEGER : rightPlaceholderEnd + 1
    }
  },

  mounted() {
    this.init()
  }
}
</script>

<style>
.indicator {
  --color-red: red;
  overflow: hidden;
}

.indicator-inner {
  display: flex;
}

.indicator-dot {
  height: var(--dot-size);
  width: calc(var(--dot-size) * 2);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.indicator-dot.active .indicator-dot-inner {
  background-color: var(--color-red);
}

.indicator-dot.placeholder .indicator-dot-inner {
  height: calc(var(--dot-size) / 2);
  width: calc(var(--dot-size) / 2);
}

.indicator-dot-inner {
  height: var(--dot-size);
  width: var(--dot-size);
  border-radius: 50%;
  background-color: #ccc;
  transition: background-color 0.3s;
}
</style>
