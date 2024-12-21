<template>
  <div class="main">
    <div class="input">
      <div class="description">Misskey帳号:</div>
      <input type="text" class="text" v-model="inputModel" />
      <div class="button" @click="convert">開始</div>
    </div>
    <div class="list">
      <div v-for="note in noteList" v-bind:key="note.id">
        <Note
          :avatarUrl="note.avatarUrl"
          :name="note.name"
          :createdAt="note.createdAt"
          :text="note.text"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="js">
import { ref } from 'vue'
import axios from 'axios'
import Note from './Note.vue'
const inputModel = ref('')
const noteList = ref(new Array())

const convert = async () => {
  const userhost = inputModel.value.split('@')
  const user = await axios.post(
    'https://' + userhost[1] + '/api/users/search-by-username-and-host',
    {
      username: userhost[0],
      host: null,
      limit: 1,
      detail: false,
    },
  )
  const notes = await axios.post('https://' + userhost[1] + '/api/users/notes', {
    userId: user.data[0].id,
    withReplies: true,
    withRenotes: false,
    withChannelNotes: true,
  })
  noteList.value.splice(0)
  for (let i = 0; i < notes.data.length; i++) {
    noteList.value.push({
      id: notes.data[i].id,
      avatarUrl: notes.data[i].user.avatarUrl,
      name: notes.data[i].user.name,
      createdAt: notes.data[i].createdAt,
      text: notes.data[i].text,
    })
  }
}
</script>

<style scoped lang="css">
.main {
  padding: 20px;
}

.input {
  overflow: hidden;
}

.input .description {
  padding-right: 10px;
  float: left;
}

.input .text {
  width: 160px;
  height: 24px;
  float: left;
}

.input .button {
  width: 60px;
  height: 24px;
  float: left;
  text-align: center;
}
</style>
