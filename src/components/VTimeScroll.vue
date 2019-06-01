<template>
    <div class="scrollContent">
        <div class="wrapperLeft wrapper" ref="wrapperLeft">
            <ul class="content contentLeft">
                <li class="option" v-for="(i, index) of h" :key="index">{{formatTime(index)}}</li>
            </ul>
        </div>
        <div class="wrapperRight wrapper" ref="wrapperRight">
            <ul class="content contentRight">
                <li class="option" v-for="(i, index) of m" :key="index">{{formatTime(i)}}</li>
            </ul>
        </div>
        <span class="colon">:</span>
        <span class="mask-top"></span>
        <span class="mask-bottom"></span>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
    name: 'vTimeScroll',
    props: {
        h: {
            type: Number,
            default: 24
        },
        m: {
            type: Array,
            default: () => {
                return [0, 30]
            }
        },
        initH: {
            type: Number,
            default: 0
        },
        initM: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            hour: this.initH,
            minute: this.initM
        }
    },
    mounted() {
        this.initPicker()
    },
    methods: {
        initPicker() {
            let _this = this
            this.$nextTick(() => {
                this.scrollLeft = new BScroll(this.$refs.wrapperLeft, {
                    scrollY: true,
                    wheel:{
                        selectedIndex: _this.initH,
                        rotate: 25,
                        adjustTime: 400,
                        wheelWrapperClass: 'wrapper',
                        wheelItemClass: 'option'
                    }
                })
                this.scrollLeft.on('scrollEnd', () => {
                    _this.hour = this.scrollLeft.selectedIndex 
                    this.$emit('selectTime', {hour: _this.hour, minute: _this.minute})
                })

                this.scrollRight = new BScroll(this.$refs.wrapperRight, {
                    scrollY: true,
                    wheel:{
                        selectedIndex: _this.initM,
                        rotate: 25,
                        adjustTime: 400,
                        wheelWrapperClass: 'wrapper',
                        wheelItemClass: 'option'
                    }
                })
                this.scrollRight.on('scrollEnd', () => {
                    _this.minute = _this.m[this.scrollRight.selectedIndex] 
                    this.$emit('selectTime', {hour: _this.hour, minute: _this.minute})
                })
            });
        },
        formatTime(timeNum) {
            return timeNum < 10 ? "0" + timeNum : timeNum + ''
        }
    }
}
</script>

<style scoped>
    * {
        margin: 0;
        padding:  0;
    }
    li {
        list-style: none;
    }
    .scrollContent {
        height: 100%;
        position: relative;
        display: flex;
        justify-content: space-around;
        overflow: hidden;
        font-size: 22px;
    }
    .wrapper {
        width: 100%;
        position: relative;
        top: 14vh;
        height: 7vh;
    }
    .content {
        text-align: center;
        line-height: 20px;
    }
    .option {
        height: 7vh;
        line-height: 7vh;
    }
    .colon {
        width: 10%;
        position: absolute;
        display: block;
        font-weight: 500;
        text-align: center;
        top: 14vh;
        line-height: 7vh;
        height: 7vh;
    }
    .mask-top, .mask-bottom {
        width: 100%;
        height: 14vh;
        background: linear-gradient(180deg,hsla(0,0%,100%,.4),hsla(0,0%,100%,.8));
        position: absolute;
        pointer-events: none;
    }
    .mask-bottom {
        bottom: 0;
        border-top: 1px solid rgba(37,38,45,.1);
    }
    .mask-top {
        top: 0;
        border-bottom: 1px solid rgba(37,38,45,.1);
    }
</style>
