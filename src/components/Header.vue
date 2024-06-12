<template>
  <header>
    <div>
      <nav>
        <button @click="view = 'daily'">일일</button>
        <button @click="view = 'monthly'">월별</button>
        <button @click="view = 'summary'">요약</button>
      </nav>
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue';

const view = ref('monthly');
</script>

<style>
header {
  background-color: #f8f9fa;
  padding: 1rem;
  text-align: center;
}
nav button {
  margin: 0 0.5rem;
}
</style>
