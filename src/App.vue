<template>
  <div id="app">
    <button @click="open">open</button>
    <TimePicker @close='closePicker()' @leftBtn="left" 
    @rightBtn="right" @badTime='badTimeTip' :isShow="isShow" :bh="8" :bm="20" :eh="10" :em="20"></TimePicker>
  </div>
</template>

<script>
import TimePicker from　'./components/TimePicker'
export default {
  name: 'app',
  data() {
    return {
      isShow: true
    }
  },
  methods: {
    open() {
      console.log('open')
      this.isShow = true
    },
    left(data) {
      console.log('left', data)
      this.isShow = false
    },
    right(data) {
      console.log('right', data)
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
