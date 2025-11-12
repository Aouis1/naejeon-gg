<template>
  <div class="w-full flex flex-col gap-4">
    <!-- Calendar Header -->
    <div class="flex items-center justify-between">
      <h1 class="text-2xl font-bold">매치 일정</h1>
      <div class="flex items-center gap-4">
        <button
          @click="changeMonth(-1)"
          class="px-4 py-2 bg-box border border-line rounded-lg hover:border-gray-600 transition-colors">
          이전
        </button>
        <span class="text-lg font-semibold">{{ currentYear }}년 {{ currentMonth }}월</span>
        <button
          @click="changeMonth(1)"
          class="px-4 py-2 bg-box border border-line rounded-lg hover:border-gray-600 transition-colors">
          다음
        </button>
      </div>
    </div>

    <!-- Calendar -->
    <div class="w-full rounded-xl overflow-hidden border border-line">
      <table class="w-full">
        <thead>
          <tr>
            <th class="p-4 text-center border-r border-b border-line bg-box">일</th>
            <th class="p-4 text-center border-r border-b border-line bg-box">월</th>
            <th class="p-4 text-center border-r border-b border-line bg-box">화</th>
            <th class="p-4 text-center border-r border-b border-line bg-box">수</th>
            <th class="p-4 text-center border-r border-b border-line bg-box">목</th>
            <th class="p-4 text-center border-r border-b border-line bg-box">금</th>
            <th class="p-4 text-center border-b border-line bg-box">토</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(week, weekIndex) in calendarWeeks" :key="weekIndex">
            <td
              v-for="(day, dayIndex) in week"
              :key="dayIndex"
              class="p-4 align-top bg-box h-32 border-b border-line"
              :class="[
                { 'bg-blue-50/5': day.isToday },
                { 'opacity-50': !day.isCurrentMonth },
                dayIndex < 6 ? 'border-r border-line' : '',
              ]">
              <div class="text-right text-2xl font-light mb-3 opacity-60">
                {{ day.date }}
              </div>
              <div v-for="event in day.events" :key="event.id" class="mb-2">
                <div class="text-xs mb-1">{{ event.time }}</div>
                <div class="font-bold text-sm" :style="{ color: event.color }">
                  {{ event.title }}
                </div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, computed } from "vue";

const currentDate = ref(new Date());

const currentYear = computed(() => currentDate.value.getFullYear());
const currentMonth = computed(() => currentDate.value.getMonth() + 1);

const changeMonth = (delta: number) => {
  const newDate = new Date(currentDate.value);
  newDate.setMonth(newDate.getMonth() + delta);
  currentDate.value = newDate;
};

// Mock events data
const mockEvents = [
  { id: 1, date: 9, time: "18:30", title: "승리", color: "#6B9CFF" },
  { id: 2, date: 10, time: "14:00", title: "패배", color: "#FF6B6B" },
  { id: 3, date: 14, time: "21:30", title: "매치 전", color: "#FFFFFF" },
  { id: 4, date: 18, time: "19:00", title: "승리", color: "#6B9CFF" },
  { id: 5, date: 22, time: "20:00", title: "매치 전", color: "#FFFFFF" },
  { id: 6, date: 25, time: "15:30", title: "패배", color: "#FF6B6B" },
];

const calendarWeeks = computed(() => {
  const year = currentDate.value.getFullYear();
  const month = currentDate.value.getMonth();
  const today = new Date();

  // 이번 달의 첫날과 마지막 날
  const firstDay = new Date(year, month, 1);
  const lastDay = new Date(year, month + 1, 0);

  // 첫 주의 시작일 (일요일부터 시작)
  const startDate = new Date(firstDay);
  startDate.setDate(startDate.getDate() - firstDay.getDay());

  // 마지막 주의 종료일 (토요일까지)
  const endDate = new Date(lastDay);
  endDate.setDate(endDate.getDate() + (6 - lastDay.getDay()));

  const weeks = [];
  const currentWeekDate = new Date(startDate);

  while (currentWeekDate <= endDate) {
    const week = [];
    for (let i = 0; i < 7; i++) {
      const date = currentWeekDate.getDate();
      const isCurrentMonth = currentWeekDate.getMonth() === month;
      const isToday =
        currentWeekDate.toDateString() === today.toDateString();

      // 해당 날짜의 이벤트 필터링
      const dayEvents = isCurrentMonth
        ? mockEvents.filter((event) => event.date === date)
        : [];

      week.push({
        date,
        isCurrentMonth,
        isToday,
        events: dayEvents,
      });

      currentWeekDate.setDate(currentWeekDate.getDate() + 1);
    }
    weeks.push(week);
  }

  return weeks;
});
</script>