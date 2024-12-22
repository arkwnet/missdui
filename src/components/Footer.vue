<template>
  <div class="footer">
    <p><b>密斯対 (MissDui)</b> - Version {{ version }}</p>
    <p>
      Copyright (c) 2024 <a href="https://arkw.net/">Sora Arakawa</a>. Licensed under the MIT
      License.
    </p>
    <p><a href="https://github.com/arkwnet/missdui">GitHub Repository</a></p>
  </div>
</template>

<script setup lang="js">
import { ref, onMounted } from 'vue'
const version = ref('')

onMounted(() => {
  version.value = import.meta.env.VITE_APP_VERSION
})
</script>

<style scoped lang="css">
.footer {
  padding: 20px;
  color: #757575;
}

.footer a {
  color: #757575;
}

.footer a:hover {
  color: #9e9e9e;
}
</style>
