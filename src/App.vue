<template>
  <div id="app">
    <button @click="open">open</button>
    <TimePicker @close='closePicker()' @leftBtn="left" 
    @rightBtn="right" @badTime='badTimeTip' :isShow="isShow" 
    :bh="8" :bm="20" :eh="10" :em="20"></TimePicker>
    <!-- 关闭时触发close事件　close事件中一定要把绑定的isShow设置为false
         点击左边按钮触发leftBtn事件　返回当前选中的时间
         点击右边按钮触发rightBtn事件　返回当前选中的时间
         isShow　属性控制picker　是否显示
         bh　bm　eh　em　分别是开始(begin)和结束时间(end)的　小时和分钟
     -->
  </div>
</template>

<script>
import TimePicker from './components/TimePicker'
export default {
  name: 'app',
  data() {
    return {
      isShow: true
    }
  },
  methods: {
    open() {
      this.isShow = true
    },
    left(data) {
      this.isShow = false
      alert(data)
    },
    right(data) {
      alert(data)
    },
    badTimeTip() {
      alert('时间不符合要求')
    },
    closePicker() {
      this.isShow = false
    }
  },
    components: {
    TimePicker
  }
}
</script>

<style>
* {
  margin: 0;
  padding:  0;
}
li {
  list-style: none;
}

</style>
