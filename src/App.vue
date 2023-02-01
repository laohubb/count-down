<script setup>
import dayjs from 'dayjs'
import {onMounted, ref} from "vue";

const today = dayjs().format('YYYY-MM-DD')
const night = dayjs(today + ' 23:00:00')
let hour=ref(0)
let minute=ref(0)

onMounted(() => {
  const computeDiff=()=>{
    let now = dayjs()
    hour.value = night.diff(now, 'hour')
    minute.value = night.diff(now, 'minute') % 60
    document.title=`还剩${hour.value}小时${minute.value}分钟`
  }
  computeDiff()
  setInterval(computeDiff, 10000)

})
</script>

<template>

  <div class="box">
    <div class="title">距离11点还有</div>
    <div class="time">
      <span class="hour">{{ hour }}</span>
      小时
      <span class="minute">{{ minute }}</span>
      分钟
    </div>
  </div>
</template>

<style scoped>
.box {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title {
  font-size: 10vw;
  margin-bottom: 10px;
}

.time {
  font-size: 10vw;
}

.hour,
.minute {
  color: #e3637c;
}
</style>
