<template>
  <div class="flex flex-col lg:flex-row w-full h-screen">
    <main class="flex-1 px-3 md:px-6 flex flex-col overflow-hidden">
      <Header />
      <div class="w-full flex-1 overflow-y-scroll py-6 md:py-10">
        <RouterView />
      </div>
    </main>
    <Sidebar />
  </div>
</template>
<script setup lang="ts">
import { RouterView } from "vue-router";
import Sidebar from "../components/Sidebar.vue";
import Header from "../components/Header.vue";
</script>
