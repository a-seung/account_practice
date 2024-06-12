<template>
  <div class="container">
    <Header />
    <!-- 월 변경 바 -->
    <div class="header">
      <button @click="prevMonth">&lt;</button>
      <div class="calendar-header">{{ year }}년 {{ month }}월</div>
      <button @click="nextMonth">&gt;</button>
    </div>
    <!-- 요일 -->
    <div class="days-of-week">
      <div class="days" v-for="(day, index) in daysOfWeek" :key="day" :class="{ sunday: index === 0, saturday: index === 6 }">{{ day }}</div>
    </div>
    <!-- 날짜 -->
    <div class="grid">
      <!-- 시작 날짜 전에 빈 칸 생성 -->
      <div class="day" v-for="blank in blankDays" :key="'blank-' + blank"></div>
      <div class="day" v-for="day in days" :key="day.date">
        <div class="date" :class="{ sunday: new Date(year, month - 1, day.date).getDay() === 0, saturday: new Date(year, month - 1, day.date).getDay() === 6 }">
          {{ day.date }}
        </div>
      </div>
      <!-- 마지막 날짜 뒤에 빈 칸 생성 -->
      <div class="day" v-for="blank in blankDaysEnd" :key="'blank-end-' + blank"></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Header from './Header.vue'; // Header 컴포넌트 가져오기

const year = ref(new Date().getFullYear());
const month = ref(new Date().getMonth() + 1);
const days = ref([]);
const blankDays = ref([]); // 달력 시작부분 빈 칸 배열 추가
const blankDaysEnd = ref([]); // 달력 끝부분 빈 칸 배열 추가
const daysOfWeek = ['일', '월', '화', '수', '목', '금', '토'];

function updateCalendar(year, month) {
  const firstDayOfMonth = new Date(year, month - 1, 1).getDay(); // 첫 번째 날의 요일 계산
  const lastDateOfMonth = new Date(year, month, 0).getDate(); // 해당 월의 마지막 날짜 계산
  const lastDayOfMonth = new Date(year, month - 1, lastDateOfMonth).getDay(); // 마지막 날의 요일 계산

  blankDays.value = Array(firstDayOfMonth).fill(null); // 빈 칸 배열 설정
  days.value = generateDays(year, month); // 날짜 배열 설정
  blankDaysEnd.value = Array(6 - lastDayOfMonth).fill(null); // 마지막 날짜 뒤의 빈 칸 배열 설정
}

function prevMonth() {
  month.value -= 1;
  if (month.value < 1) {
    month.value = 12;
    year.value -= 1;
  }
  updateCalendar(year.value, month.value); // 달력 업데이트 함수 호출
}

function nextMonth() {
  month.value += 1;
  if (month.value > 12) {
    month.value = 1;
    year.value += 1;
  }
  updateCalendar(year.value, month.value); // 달력 업데이트 함수 호출
}

function generateDays(year, month) {
  const date = new Date(year, month - 1, 1);
  const days = [];
  while (date.getMonth() === month - 1) {
    days.push({
      date: date.getDate(),
      // income: Math.floor(Math.random() * 10000),
      // expense: Math.floor(Math.random() * 10000),
    });
    date.setDate(date.getDate() + 1);
  }
  return days;
}
updateCalendar(year.value, month.value); // 초기 달력 설정
</script>

<style scoped>
.container {
  max-width: 375px;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.calendar-header {
  font-weight: bold;
}

.days-of-week {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  font-size: 10px;
  height: 30px;
  text-align: center;
}

.days {
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 5px;
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: center;
  height: 30px;
  font-weight: bold;
}
.sunday {
  color: red;
}
.saturday {
  color: blue;
}

.grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day {
  background-color: #fff;
  border: 1px solid #ccc;
  padding: 5px;
  text-align: center;
  height: 80px;
  font-weight: bold;
}

.date {
  font-weight: 500;
  font-size: 10px;
  text-align: left;
}
</style>
