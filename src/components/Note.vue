<template>
  <img class="avatar" :src="avatarUrl" />
  <div class="name">{{ name }}</div>
  <div class="created-at">{{ createdAt }}</div>
  <div class="text">{{ text }}</div>
</template>

<script setup lang="js">
import { defineProps } from 'vue'
const props = defineProps(['avatarUrl', 'name', 'createdAt', 'text'])
</script>
