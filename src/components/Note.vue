<template>
  <div class="note">
    <div class="left">
      <img class="avatar" :src="avatarUrl" />
    </div>
    <div class="right">
      <div class="name">{{ name }}</div>
      <div class="created-at">{{ createdAt }}</div>
      <div class="text">{{ text }}</div>
    </div>
  </div>
</template>

<script setup lang="js">
import { defineProps } from 'vue'
const props = defineProps(['avatarUrl', 'name', 'createdAt', 'text'])
</script>

<style scoped lang="css">
.note {
  margin-bottom: 2px;
  padding: 20px 15px;
  overflow: hidden;
  background-color: #fff;
}

.note .left {
  width: 80px;
  float: left;
}

.note .left .avatar {
  width: 60px;
  height: 60px;
  margin-left: 10px;
  border-radius: 30px;
}

.note .right {
  width: calc(100% - 80px);
  padding-left: 10px;
  float: right;
}

.note .right .name {
  color: #212121;
  font-weight: bold;
}

.note .right .created-at {
  color: #424242;
}

.note .right .text {
  color: #212121;
}
</style>
