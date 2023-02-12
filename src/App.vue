<script setup>
import dayjs from 'dayjs'
import { onMounted, ref } from "vue";
import Modal from './components/Modal.vue'

let customHour = ref('23')
let customMinute = ref('44')
const today = dayjs().format('YYYY-MM-DD')


let hour = ref(0)
let minute = ref(0)
let visible = ref(false)
onMounted(() => {
  getTime()

  computeDiff()

  setInterval(computeDiff, 10000)

})
const closeModal = () => {
  localStorage.setItem('customHour', formatNumber(customHour.value))
  localStorage.setItem('customMinute', formatNumber(customMinute.value))
  computeDiff()
  visible.value = false
}
const formatNumber = (input) => {
  input = parseInt(input)
  if (typeof input === "number") {
    if (input >= 0 && input < 10) {
      return "0" + input.toString();
    } else {
      return input.toString();
    }
  } else {
    return "00";
  }
}
const getTime = () => {
  let hour = localStorage.getItem('customHour')
  let minute = localStorage.getItem('customMinute')

  if (hour && minute) {
    customHour.value = hour
    customMinute.value = minute
  }

}
const computeDiff = () => {
  let now = dayjs()
  let customTime = dayjs(today + ` ${customHour.value}:${customMinute.value}:00`)
  hour.value = customTime.diff(now, 'hour')
  minute.value = customTime.diff(now, 'minute') % 60
  document.title = `${hour.value}h${minute.value}m`
}
</script>

<template>

  <div class="box" @click="visible = true">

    <div class="time">
      <div class="title">距离指定时间还有</div>
      <span class="hour">{{ hour }}</span>
      小时
      <span class="minute">{{ minute }}</span>
      分钟
    </div>
  </div>
  <div class="hello">

    <modal v-if="visible" @close="closeModal">
      <template v-slot:header>
        <h3>自定义时间</h3>
      </template>
      <template v-slot:body>
        <input type="number" v-model="customHour">:
        <input type="number" v-model="customMinute">
      </template>
    </modal>
  </div>
</template>

<style scoped>

.box {
  height: 99vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}

.title {
  font-size: 2vw;
  margin-bottom: 10px;
  position: absolute;
  top: -30px;
  left: 0;
}

.time {
  font-size: 10vw;

  position: relative;
}

.hour,
.minute {
  color: #e3637c;
}
</style>
