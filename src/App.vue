<template>
  <div class="clock-container">
    <div class="clock-digital">
      <div class="date">{{ date }}</div>
      <div class="time">{{ time }}</div>
      <div class="day">{{ day }}</div>
    </div>
    <div class="clock-analog">
      <div class="spear"></div>
      <div class="hour" :style="{ transform: `rotate(${hourDeg}deg)` }">
        <span
          v-for="i in 12"
          :key="i"
          :style="{ transform: `rotate(${30 * i}deg) translateX(100px)` }"
        >
          {{ i }}
        </span>
      </div>
      <div class="minute" :style="{ transform: `rotate(${minuteDeg}deg)` }">
        <span
          v-for="i in 60"
          :key="i"
          :style="{ transform: `rotate(${6 * (i - 1)}deg) translateX(145px)` }"
        >
          {{ i - 1 }}
        </span>
      </div>
      <div class="second" :style="{ transform: `rotate(${secondDeg}deg)` }">
        <span
          v-for="i in 60"
          :key="i"
          :style="{ transform: `rotate(${6 * (i - 1)}deg) translateX(195px)` }"
        >
          {{ i - 1 }}
        </span>
      </div>
      <div class="dail">
        <span
          v-for="i in 60"
          :key="i"
          :style="{ transform: `rotate(${6 * (i - 1)}deg) translateX(${230}px)` }"
        >
          {{ i - 1 }}
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { readonly, ref } from 'vue'
import dayjs from 'dayjs'

const monthsMap = readonly([
  'January',
  'February',
  'March',
  'April',
  'May',
  'June',
  'July',
  'August',
  'September',
  'October',
  'November',
  'December'
])
const daysMap = readonly([
  'Sunday',
  'Monday',
  'Tuesday',
  'Wednesday',
  'Thursday',
  'Friday',
  'Saturday'
])

const time = ref('')
const day = ref('')
const date = ref('')
const secondDeg = ref(0)
const minuteDeg = ref(0)
const hourDeg = ref(0)

function getTime() {
  const now = dayjs()
  const second = now.get('second')
  const minute = now.get('minute')
  const hour = now.get('hour')
  const now_time = now.format('h:mm A')
  const now_day = now.get('day')
  const now_month = now.get('month')
  const now_date = now.get('date')

  time.value = now_time
  day.value = daysMap[now_day]
  date.value = `${now_date} . ${monthsMap[now_month]}`
  secondDeg.value = second * -6
  minuteDeg.value = minute * -6
  hourDeg.value = hour * -30
}

getTime()
setInterval(getTime, 1000)
</script>

<style lang="scss" scoped>
.clock-container {
  width: 480px;
  height: 480px;
  box-shadow: 0 0 25px 3px #000, 0 0 10px rgba(0, 0, 0, 0.8) inset;
  @apply bg-clock rounded-full overflow-hidden relative;

  .clock-digital {
    position: absolute;
    z-index: 200;
    height: 444px;
    width: 224px;
    background: #090909;
    left: 18px;
    top: 18px;
    border-radius: 220px 0 0 220px;
    box-shadow: 5px 0 15px #000;

    &:after {
      content: '';
      position: absolute;
      border: 15px solid #8e0a0a;
      border-right: none;
      height: 400px;
      width: 200px;
      border-radius: 220px 0 0 220px;
      left: 25px;
      top: 25px;
    }

    .time {
      background: #111;
      position: absolute;
      right: 20px;
      top: 50%;
      transform: translateY(-50%);
      color: #fff;
      border-radius: 50px;
      font-size: 24px;
      padding: 2px 20px;
      box-shadow: 0 0 15px #000 inset;
    }

    .day {
      background: #111;
      position: absolute;
      right: 20px;
      bottom: 100px;
      color: #fff;
      border-radius: 20px;
      box-shadow: 0 0 15px #000 inset;
      padding: 2px 20px;
      font-size: 16px;
    }

    .date {
      background: #111;
      position: absolute;
      right: 20px;
      top: 100px;
      color: #fff;
      border-radius: 20px;
      font-size: 16px;
      box-shadow: 0 0 10px #000 inset;
      padding: 2px 20px;
    }
  }

  .clock-analog {
    width: 440px;
    height: 440px;
    border-radius: 50%;
    margin: 20px;
    background: #fff;
    z-index: 5;
    position: relative;
    box-shadow: 0 0 25px 3px #000 inset;
    .spear {
      position: absolute;
      width: 220px;
      height: 1px;
      background: red;
      left: 50%;
      top: 50%;
      transform: translateY(-50%);
      z-index: 200;
      box-shadow: 0 3px 3px rgba(0, 0, 0, 0.4);

      &:after {
        content: '';
        position: absolute;
        border: 7px solid transparent;
        border-right-color: red;
        right: 0;
        top: -7px;
      }

      &:before {
        content: '';
        position: absolute;
        border: 7px solid transparent;
        border-left-color: red;
        left: 2px;
        top: -7px;
      }
    }

    .second,
    .minute,
    .hour {
      width: 20px;
      height: 20px;
      position: absolute;
      left: 0;
      top: 0;
      right: 0;
      bottom: 0;
      margin: auto;
      z-index: 100;
      transition: 0.2s 0.2s ease-in;
      transform: rotate(90deg);

      span {
        position: absolute;
        width: 20px;
        height: 20px;
        line-height: 20px;
        text-align: center;
        transform-origin: 50%;
        z-index: 5;

        &:after {
          content: '';
          width: 4px;
          height: 1px;
          background: #000;
          position: absolute;
          left: 130%;
          top: -10%;
          opacity: 0.3;
        }

        &:nth-child(5n + 2):after {
          width: 7px;
          opacity: 1;
          left: 110%;
        }
      }
    }

    .hour {
      z-index: 150;
      font-size: 32px;
      font-weight: 400;

      &:after {
        content: '';
        background: #fff;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        width: 250px;
        height: 250px;
        border-radius: 50%;
        box-shadow: 0 0 15px 2px rgba(0, 0, 0, 0.6) inset;
      }

      span:after {
        opacity: 1;
        width: 4px;
        height: 1px;
        color: #666;
        transform: translate(5px, 12px);
      }
    }

    .minute {
      color: #fff;
      font-size: 16px;

      &:after {
        content: '';
        background: #333;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        width: 350px;
        height: 350px;
        border-radius: 50%;
        box-shadow: 0 0 25px 2px #000 inset;
      }

      span:after {
        background: #fff;
        transform: translate(10px, -7px) rotate(-9deg);
      }
    }

    .second {
      font-size: 12px;

      span:after {
        transform: translate(5px, -10px);
      }
    }

    .dail {
      width: 20px;
      height: 20px;
      position: absolute;
      left: 0;
      top: 0;
      right: 0;
      bottom: 0;
      margin: auto;
      z-index: 100;

      span {
        width: 20px;
        height: 20px;
        line-height: 20px;
        transform-origin: 50%;
        text-indent: 1000px;
        overflow: hidden;
        position: absolute;

        &:after {
          content: '';
          position: absolute;
          width: 4px;
          height: 4px;
          border-radius: 50%;
          background-color: #7d7e7d;
          background-size: 100%;
          background-image: linear-gradient(to right, #7d7e7d 0%, #0e0e0e 100%);
          left: 50%;
          top: 50%;
          transform: translate(-50%, -50%);
        }

        &:nth-child(5n + 1):after {
          width: 8px;
        }
      }
    }
  }
}
</style>
