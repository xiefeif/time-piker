<template>
    <div :class="[{mask: isShow}, {fade: !isShow}]">
        <div :class="[{'picker': isShow}, {pickerDown: !isShow}]">
            <div class="header">
                <span @click="leftBtnClick" class="leftBtn">{{leftBtnName}}</span>
                <span class="title">{{title}}</span>
                <span @click="rightBtnClick" class="rightBtn">{{rightBtnName}}</span>
            </div>
            <div class="content">
                <div class="select leftSelect">
                    <p>开始时间</p>
                    <VTimePicker :initH="bh" :initM="bm" @selectTime="beginTime"  class="timePicker"></VTimePicker>
                </div>
                <div class="select rightSelect">
                    <p>结束时间</p>
                    <VTimePicker :initH="eh" :initM="em" @selectTime="endTime" class="timePicker"></VTimePicker>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import VTimePicker from './VTimeScroll'
export default {
    name: "timePicker",
    data() {
        return {
            begin: {
                hour: this.bh,
                minute: this.bm
            },
            end: {
                hour: this.eh,
                minute: this.em
            }
        }
    },
    props: {
        leftBtnName: {
            type: String,
            default: "取消"
        },
        rightBtnName: {
            type: String,
            default: "确定"
        },
        title: {
            type: String,
            default: "选择营业时间"
        },
        isShow: {
            type: Boolean,
            default: false
        },
        bh: {
            type: Number,
            default: 0
        },
        bm: {
            type: Number,
            default: 0
        },
        eh: {
            type: Number,
            default: 0
        },
        em: {
            type: Number,
            default: 0
        }
    },
    methods: {
        beginTime(data) {
            this.begin.hour = data.hour
            this.begin.minute = data.minute
        },
        endTime(data) {
            this.end.hour = data.hour
            this.end.minute = data.minute
        },
        close() {
            this.$emit('close')
        },
        leftBtnClick() {
            this.$emit('leftBtn', {begin: this.begin, end: this.end})
            this.close()
        },
        rightBtnClick() {
            if (this.checkRightTime(this.begin, this.end)) {
                return
            }
            this.$emit('rightBtn', {begin: this.begin, end: this.end})
            this.close()
        },
        checkRightTime(begin, end) {
            let bHour = parseInt(begin.hour)
            let bMinute =parseInt(begin.minute)
            let eHour = parseInt(end.hour)
            let eMinute = parseInt(end.minute)
            if (bHour > eHour || (eHour - bHour < 1) || (eHour - bHour == 1 && bMinute < eMinute)) {
                // 开始时间大于结束时间 或者在1小时内
                this.$emit('badTime')
                return true
            } 
        }
    },
    components: {
        VTimePicker
    }
}
</script>

<style scoped>
    .mask {
        width: 100%;
        height: 100vh;
        position: fixed;
        top: 0;
        background: rgba(0, 0, 0, .3);
        animation: mask 0.4s forwards;
    }
    .picker {
        width: 100%;
        height: 50vh;
        position: fixed;
        background: white;
        animation: pickerUp .4s forwards;
    }
    .header {
        display: flex;
        justify-content: space-around;
        width: 100%;
        height: 7vh;
        border-bottom: 1px solid grey;
        line-height: 8vh;
        font-weight: 500;
    }
    .rightBtn {
        color: orangered;
    }
    .picker .content{
        display: flex;
        justify-content: space-around;
        z-index: 9999;
    }
    .picker .content .select{
        height: 42vh;
        text-align: center;
        width: 100%;
    }
    .picker .content p{
        height: 5vh;
        line-height: 6vh;
    }
    .timePicker {
        height: 35vh;
    }
    .pickerDown {
        width: 100%;
        height: 50vh;
        position: fixed;
        background: white;
        animation: pickerDown 0.4s forwards;
    }
    .fade {
        width: 100%;
        height: 100vh;
        position: fixed;
        top: 0;
        background: rgba(0, 0, 0, .3);
        animation: fade .4s forwards;
    }
    @keyframes mask {
        from {
            background: rgba(0, 0, 0, 0);
        }
        to {
            background: rgba(0, 0, 0, .3);
            z-index: 9998;
        }
    }
    @keyframes fade {
        from {
            background: rgba(0, 0, 0, .3);
        }
        to {
            background: rgba(0, 0, 0, 0);
            z-index: -99999;
        }
    }
    @keyframes pickerDown {
        from {
            bottom: 0;
        }
        to {
            bottom: -50vh;
        }
    }
    @keyframes pickerUp {
        from {
            bottom: -50vh;
        }
        to {
            bottom: 0;
        }
    }
</style>
