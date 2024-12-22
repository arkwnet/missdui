<template>
  <div class="main">
    <div class="information">
      将Misskey筆記転換為偽中国語的工具。輸入Misskey号、然後按「表示」。
    </div>
    <div class="input">
      <div class="description">Misskey号 (例: @arkw@misskey.io)</div>
      <input type="text" class="text" v-model="inputModel" />
      <div class="button" @click="convert">表示</div>
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
const regexpKanji =
  /^([\u{3005}\u{3007}\u{303b}\u{3400}-\u{9FFF}\u{F900}-\u{FAFF}\u{20000}-\u{2FFFF}][\u{E0100}-\u{E01EF}\u{FE00}-\u{FE02}]?)+$/mu
const inputModel = ref('')
const noteList = ref(new Array())

const convert = async () => {
  const userhost = inputModel.value.split('@')
  const user = await axios.post(
    'https://' + userhost[2] + '/api/users/search-by-username-and-host',
    {
      username: userhost[1],
      host: userhost[2],
      limit: 1,
      detail: false,
    },
  )
  const notes = await axios.post('https://' + userhost[2] + '/api/users/notes', {
    userId: user.data[0].id,
    withReplies: true,
    withRenotes: false,
    withChannelNotes: true,
    limit: 30,
  })
  noteList.value.splice(0)
  for (let i = 0; i < notes.data.length; i++) {
    const textArray = Array.from(notes.data[i].text)
    let result = ''
    for (let i = 0; i < textArray.length; i++) {
      if (regexpKanji.test(textArray[i]) == true) {
        result += textArray[i]
      }
    }
    if (result != '') {
      noteList.value.push({
        id: notes.data[i].id,
        avatarUrl: notes.data[i].user.avatarUrl,
        name: notes.data[i].user.name,
        createdAt: notes.data[i].createdAt,
        text: result,
      })
    }
  }
}
</script>

<style scoped lang="css">
.main {
  padding: 20px;
}

.main .information {
  margin-bottom: 10px;
  padding: 10px;
  color: #757575;
  background-color: #b3e5fc;
  border-radius: 5px;
}

.main .input {
  overflow: hidden;
}

.main .input .description {
  padding-right: 10px;
  padding-bottom: 4px;
  color: #757575;
}

.main .input .text {
  width: 200px;
  height: 32px;
  margin-right: 10px;
  padding-left: 4px;
  float: left;
  border: 0;
  border-radius: 4px;
  outline: 0;
}

.main .input .text:focus {
  border: 1px solid #8bc34a;
  outline: 0;
}

.main .input .button {
  width: 100px;
  height: 32px;
  padding: 4px 0;
  color: #fff;
  background: linear-gradient(to top, #0ba360 0%, #3cba92 100%);
  border-radius: 16px;
  float: left;
  text-align: center;
}

.main .input .button:hover {
  background: #8bc34a;
  cursor: pointer;
}

.main .list {
  margin-top: 15px;
}
</style>
