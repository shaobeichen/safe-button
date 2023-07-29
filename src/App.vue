<template>
  <div class="page">
    <div class="container">
      <div class="box">
        <div class="door">
          <div class="gear" :style="gearStyle">
            <div class="button"></div>
          </div>
        </div>
      </div>
    </div>
    <div class="slider">
      <el-slider v-model="count" :min="min" :max="max" vertical height="200px" />
    </div>
  </div>
</template>

<script>
import * as Tone from 'tone'
Tone.start()
const player = new Tone.Player({
  url: 'button.wav'
}).toDestination()

export default {
  data() {
    return {
      count: 180,
      min: 0,
      max: 360
    }
  },
  computed: {
    gearStyle() {
      return {
        transform: `rotate(${this.count}deg)`
      }
    }
  },
  watch: {
    count() {
      try {
        player.start()
      } catch (error) {
        /* empty */
      }
    }
  },
  mounted() {
    setTimeout(() => {
      this.init()
    }, 1000)
  },
  methods: {
    init() {
      this.mouse(
        () => {
          this.change()
        },
        () => {
          this.change(true)
        }
      )
    },
    change(plus) {
      if (this.count === this.min && plus) this.count++
      else if (this.count === this.max) this.count--
      else if ((this.count > this.min && this.count < this.max) & plus) this.count++
      else if (this.count > this.min && this.count < this.max) this.count--
    },
    mouse(upward, downward) {
      // 兼容不同浏览器的鼠标滚轮事件写法
      function handleMouseWheel(event) {
        // 使用event.wheelDelta获取滚动方向（仅适用于部分浏览器）
        var delta = event.wheelDelta || -event.detail

        // 根据滚动方向执行相应操作
        if (delta > 0) {
          // 鼠标向上滚动
          // 执行你的操作
          upward()
        } else {
          // 鼠标向下滚动
          // 执行你的操作
          downward()
        }
      }

      // 添加鼠标滚轮事件监听器
      if (window.addEventListener) {
        // 支持标准浏览器
        window.addEventListener('mousewheel', handleMouseWheel, false)
        window.addEventListener('DOMMouseScroll', handleMouseWheel, false)
      } else {
        // 兼容IE浏览器
        window.attachEvent('onmousewheel', handleMouseWheel)
      }
    }
  }
}
</script>

<style scoped lang="scss">
.page {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.box {
  position: relative;
}
.door {
  position: relative;
  width: 300px;
  height: 450px;
  background: #828282;
  display: flex;
  justify-content: center;
  border-radius: 10px;
}
.gear {
  position: absolute;
  top: 80px;
  width: 100px;
  height: 100px;
  background: #494949;
  border-radius: 50%;
  transform-origin: center;
  border: 10px solid #e7e7e7;
  display: flex;
  justify-content: center;
  align-items: center;
}
.button {
  width: 50%;
  height: 50%;
  background: #1f1f1f;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  &:after {
    content: '';
    display: block;
    width: 60%;
    height: 60%;
    border-radius: 50%;
    border: 1px solid #565656;
  }
}
.slider {
  margin: 0 100px;
}
</style>
