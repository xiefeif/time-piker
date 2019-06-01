<template>
    <div class="scrollContent">
        <div class="wrapperLeft wrapper" ref="wrapperLeft">
            <ul class="content contentLeft">
                <li :class="['option', {selected: hour == index}]" v-for="(i, index) of h" :key="index">{{index < 10 ? "0" + index : index}}</li>
            </ul>
        </div>
        <div class="wrapperRight wrapper" ref="wrapperRight">
            <ul class="content contentRight">
                <li :class="['option', {selected: minute == index}]" v-for="(i, index) of m" :key="index">{{index < 10 ? "0" + index : index}}</li>
            </ul>
        </div>
        <span class="colon">:</span>
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
            type: Number,
            default: 60
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
            this.scrollLeft.on('scrollEnd', (event) => {
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
            this.scrollRight.on('scrollEnd', (event) => {
                _this.minute = this.scrollRight.selectedIndex 
                this.$emit('selectTime', {hour: _this.hour, minute: _this.minute})
            })
        });
    },
}
</script>

<style scoped>
    .scrollContent {
        height: 100%;
        position: relative;
        display: flex;
        justify-content: space-around;
        overflow: hidden;
        font-size: 22px;
        color: grey;
    }
    .wrapper {
        width: 100%;
        position: relative;
        top: 16vh;
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
    .selected {
        color: black;
    }
    .colon {
        width: 10%;
        position: absolute;
        display: block;
        font-weight: 500;
        text-align: center;
        top: 16vh;
        line-height: 7vh;
        height: 7vh;
        color: black;
    }
</style>
