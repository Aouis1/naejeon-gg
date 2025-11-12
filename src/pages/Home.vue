<template>
  <div class="w-full flex flex-col items-start px-6 gap-20">
    <div class="w-full flex flex-col gap-2">
      <div class="w-full max-w-240 flex items-end justify-between">
        <h1 class="text-xl font-semibold">이번주 매치 일정</h1>
        <router-link
          to="/schedule"
          class="text-sm text-gray-400 inline-flex items-center group transition-colors hover:text-gray-300">
          <span>전체 일정 보기</span>
          <ChevronRight :size="20" />
        </router-link>
      </div>
      <div class="w-full max-w-240 overflow-x-scroll scrollbar-hide">
        <div class="w-240 rounded-xl overflow-hidden border border-line">
          <table>
            <thead>
              <tr>
                <th
                  class="p-2 text-center border-r border-b border-line bg-box w-37.5">
                  일
                </th>
                <th
                  class="p-2 text-center border-r border-b border-line bg-box w-37.5">
                  월
                </th>
                <th
                  class="p-2 text-center border-r border-b border-line bg-box w-37.5">
                  화
                </th>
                <th
                  class="p-2 text-center border-r border-b border-line bg-box w-37.5">
                  수
                </th>
                <th
                  class="p-2 text-center border-r border-b border-line bg-box w-37.5">
                  목
                </th>
                <th
                  class="p-2 text-center border-r border-b border-line bg-box w-37.5">
                  금
                </th>
                <th class="p-2 text-center border-b border-line bg-box w-37.5">
                  토
                </th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td
                  v-for="(day, index) in weekDays"
                  :key="day.date"
                  class="p-4 align-top bg-box h-[150px] w-37.5"
                  :class="[
                    { 'bg-blue-50/5': day.isToday },
                    index < 6 ? 'border-r border-line' : '',
                  ]">
                  <div class="text-right text-3xl font-light mb-4 opacity-60">
                    {{ day.dateNum }}
                  </div>
                  <div v-for="event in day.events" :key="event.id" class="mb-3">
                    <div class="text-sm mb-1">{{ event.time }}</div>
                    <div class="font-bold" :style="{ color: event.color }">
                      {{ event.title }}
                    </div>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- <button class="w-full py-3 bg-box border border-line rounded-lg cursor-pointer">더보기</button> -->
    </div>

    <div class="w-full max-w-320 flex flex-col gap-2">
      <div class="w-full flex items-end justify-between">
        <h1 class="text-xl font-semibold">최근 전적</h1>
        <router-link
          to="/history"
          class="text-sm text-gray-400 transition-colors hover:text-gray-300">
          더보기
        </router-link>
      </div>
      <div class="space-y-2">
        <MatchItem
          v-for="match in matchHistory"
          :key="match.id"
          v-bind="match" />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, computed } from "vue";
import MatchItem from "../components/MatchItem.vue";
import { ChevronRight } from "lucide-vue-next";

const today = ref(new Date());

const mockEvents = [
  { id: 1, date: 9, time: "18:30 시작", title: "승리", color: "#6B9CFF" },
  { id: 2, date: 10, time: "14:00 시작", title: "패배", color: "#FF6B6B" },
  { id: 3, date: 14, time: "21:30 시작", title: "매치 전", color: "#FFFFFF" },
];

const weekDays = computed(() => {
  const days = [];
  const currentDay = today.value.getDay(); // 0(일) ~ 6(토)
  const startOfWeek = new Date(today.value);
  startOfWeek.setDate(today.value.getDate() - currentDay);

  const dayNames = ["일", "월", "화", "수", "목", "금", "토"];

  for (let i = 0; i < 7; i++) {
    const date = new Date(startOfWeek);
    date.setDate(startOfWeek.getDate() + i);

    const isToday = date.toDateString() === today.value.toDateString();
    const dateNum = date.getDate();

    // 해당 날짜의 이벤트 필터링
    const dayEvents = mockEvents.filter((event) => event.date === dateNum);

    days.push({
      day: dayNames[i],
      date: date.getTime(),
      dateStr: `${date.getMonth() + 1}/${date.getDate()}`,
      dateNum: dateNum,
      isToday,
      events: dayEvents,
    });
  }

  return days;
});

const matchHistory = ref([
  {
    id: 1,
    isVictory: false,
    gameType: "Ranked Solo/Duo",
    time: "3 days ago",
    duration: "34m 46s",
    kills: 7,
    deaths: 6,
    assists: 3,
    kda: "1.67:1",
    rank: "7th",
    tier: "Gold 2",
    level: 17,
    mapName: "Laning 51 : 49",
    stats: {
      pkill: 32,
      cs: 276,
      csPerMin: "7.9",
    },
    badges: {
      doubleKill: true,
      struggle: true,
    },
  },
  {
    id: 2,
    isVictory: true,
    gameType: "Ranked Solo/Duo",
    time: "3 days ago",
    duration: "24m 52s",
    kills: 11,
    deaths: 6,
    assists: 11,
    kda: "3.67:1",
    rank: "2nd",
    tier: "Gold 2",
    level: 14,
    mapName: "Laning 44 : 56",
    stats: {
      pkill: 58,
      cs: 173,
      csPerMin: "7",
    },
    badges: {},
  },
  {
    id: 3,
    isVictory: true,
    gameType: "Ranked Solo/Duo",
    time: "4 days ago",
    duration: "28m 15s",
    kills: 9,
    deaths: 4,
    assists: 8,
    kda: "4.25:1",
    rank: "3rd",
    tier: "Gold 2",
    level: 15,
    mapName: "Laning 48 : 52",
    stats: {
      pkill: 45,
      cs: 215,
      csPerMin: "7.6",
    },
    badges: {
      doubleKill: true,
    },
  },
  {
    id: 4,
    isVictory: false,
    gameType: "Ranked Solo/Duo",
    time: "5 days ago",
    duration: "31m 22s",
    kills: 5,
    deaths: 8,
    assists: 6,
    kda: "1.38:1",
    rank: "6th",
    tier: "Gold 2",
    level: 16,
    mapName: "Laning 46 : 54",
    stats: {
      pkill: 28,
      cs: 198,
      csPerMin: "6.3",
    },
    badges: {
      struggle: true,
    },
  },
  {
    id: 5,
    isVictory: true,
    gameType: "Ranked Solo/Duo",
    time: "5 days ago",
    duration: "26m 38s",
    kills: 13,
    deaths: 3,
    assists: 9,
    kda: "7.33:1",
    rank: "1st",
    tier: "Gold 2",
    level: 14,
    mapName: "Laning 42 : 58",
    stats: {
      pkill: 62,
      cs: 187,
      csPerMin: "7.0",
    },
    badges: {
      doubleKill: true,
    },
  },
]);
</script>
