<template>
  <div
    class="rounded-lg overflow-hidden border-l-8 flex"
    :class="[
      isVictory ? 'border-l-win bg-[#28344E]' : 'border-l-defeat bg-[#59343B]'
    ]">
    <!-- Left Section -->
    <div class="shrink-0 w-36 p-3 flex flex-col justify-between">
      <div>
        <div
          class="text-sm font-semibold"
          :class="isVictory ? 'text-win' : 'text-defeat'">
          {{ gameType }}
        </div>
        <div class="text-gray-400 text-xs mt-1">{{ time }}</div>
      </div>
      <div class="mt-2">
        <div class="text-sm font-semibold" :class="isVictory ? 'text-win' : 'text-white'">
          {{ isVictory ? 'Victory' : 'Defeat' }}
        </div>
        <div class="text-gray-400 text-xs mt-1">{{ duration }}</div>
      </div>
    </div>

    <!-- Champion & Items -->
    <div class="flex items-center p-3 flex-1 gap-3">
      <!-- Champion Image with Level -->
      <div class="relative shrink-0 w-[70px]">
        <div class="w-14 h-14 rounded-full bg-gray-700 flex items-center justify-center">
          <span class="text-gray-400 text-xs">Champion</span>
        </div>
        <div class="absolute bottom-0 right-0 w-5 h-5 rounded-full bg-gray-800 flex items-center justify-center text-white text-xs font-bold">
          {{ level }}
        </div>
      </div>

      <!-- Spells & Rune -->
      <div class="flex flex-col gap-1 w-[60px] shrink-0">
        <div class="flex gap-1">
          <div class="w-6 h-6 rounded bg-gray-700"></div>
          <div class="w-6 h-6 rounded bg-gray-700"></div>
        </div>
        <div class="flex gap-1">
          <div class="w-6 h-6 rounded-full bg-gray-700"></div>
          <div class="w-6 h-6 rounded bg-gray-700"></div>
        </div>
      </div>

      <!-- KDA -->
      <div class="flex flex-col justify-center w-[100px] shrink-0">
        <div class="flex items-center gap-1 mb-1">
          <span class="text-white text-lg font-bold">{{ kills }}</span>
          <span class="text-gray-400 text-sm">/</span>
          <span class="text-defeat text-lg font-bold">{{ deaths }}</span>
          <span class="text-gray-400 text-sm">/</span>
          <span class="text-white text-lg font-bold">{{ assists }}</span>
        </div>
        <div class="text-gray-400 text-xs">{{ kda }} KDA</div>
      </div>

      <!-- Items -->
      <div class="flex gap-1 w-55 shrink-0">
        <div v-for="i in 6" :key="i" class="w-7 h-7 rounded bg-gray-700"></div>
        <div class="w-7 h-7 rounded-full bg-yellow-600 flex items-center justify-center">
          <span class="text-xs font-bold">0</span>
        </div>
      </div>

      <!-- Badges -->
      <div class="flex flex-wrap gap-2 w-50 shrink-0">
        <span
          v-if="badges.doubleKill"
          class="bg-defeat text-white px-3 py-1 rounded-full text-xs font-bold">
          Double Kill
        </span>
        <span class="bg-gray-600 text-gray-300 px-3 py-1 rounded-full text-xs font-semibold">
          {{ rank }}
        </span>
        <span
          v-if="badges.struggle"
          class="bg-gray-600 text-gray-300 px-3 py-1 rounded-full text-xs font-semibold">
          Struggle
        </span>
      </div>

      <!-- Map & Stats -->
      <div class="text-right w-32 shrink-0">
        <div class="text-defeat text-sm font-semibold">{{ mapName }}</div>
        <div class="text-gray-400 text-xs mt-1">P/Kill {{ stats.pkill }}%</div>
        <div class="text-gray-400 text-xs">CS {{ stats.cs }} ({{ stats.csPerMin }})</div>
        <div class="flex items-center gap-1 mt-1 justify-end">
          <div class="w-4 h-4 bg-yellow-600 rounded-sm"></div>
          <span class="text-white text-xs font-semibold">{{ tier }}</span>
        </div>
      </div>

      <!-- Team Players -->
      <div class="flex flex-col gap-0.5 w-30 shrink-0">
        <div v-for="i in 5" :key="i" class="flex items-center gap-2">
          <div class="w-5 h-5 rounded-sm bg-gray-700"></div>
          <span class="text-gray-400 text-xs truncate">Player {{ i }}</span>
        </div>
      </div>

      <!-- Enemy Players -->
      <div class="flex flex-col gap-0.5 w-30 shrink-0">
        <div v-for="i in 5" :key="i" class="flex items-center gap-2">
          <div class="w-5 h-5 rounded-sm bg-gray-700"></div>
          <span class="text-gray-400 text-xs truncate">Player {{ i }}</span>
        </div>
      </div>

      <!-- Expand Icon -->
      <button
        class="w-10 shrink-0 flex justify-center"
        :class="isVictory ? 'text-win' : 'text-defeat'">
        <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20">
          <path
            d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
interface MatchItemProps {
  isVictory: boolean;
  gameType: string;
  time: string;
  duration: string;
  kills: number;
  deaths: number;
  assists: number;
  kda: string;
  rank: string;
  tier: string;
  level?: number;
  rankGrade?: string;
  score?: number;
  mapName: string;
  teamScore?: number;
  enemyScore?: number;
  stats: {
    pkill?: number;
    cs?: number;
    csPerMin?: string;
    kd?: string;
    dda?: string;
    acs?: number;
    hs?: number;
    adr?: number;
  };
  badges: {
    clutch?: number;
    doubleKill?: boolean;
    struggle?: boolean;
  };
}

defineProps<MatchItemProps>();
</script>
