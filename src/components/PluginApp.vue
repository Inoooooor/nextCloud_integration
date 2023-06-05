<script setup>
// import HDE from '../plugin'
import axios from 'axios'
import { ref } from 'vue'

const selectedFile = ref(null)
const sendFilesArray = async () => {
  const postUrl = 'https://devtest.helpdeskeddy.com/api/v2/tickets/132/posts/'
  const postData = { text: 'FROMAXIOS', files: [selectedFile.value, null] }

  const config = {
    headers: {
      'Content-Type': 'multipart/form-data',
    },
    auth: {
      username: '1aynur1@mail.ru',
      password: '6d0caa1a-8869-494d-8d50-b076436811cf',
    },
  }
  try {
    const response = await axios.post(postUrl, postData, config)
    console.log(response.data)
  } catch (error) {
    console.log(error)
  }
}

const onFileSelected = (event) => {
  const file = event.target.files[0]
  selectedFile.value = file
  console.log('file changed', selectedFile.value)
}

// sendFilesArray()
</script>

<template>
  <form id="cloudForm" prevent>
    <label for="file">В облако </label>
    <input id="file" type="file" name="file" @change="onFileSelected" />
    <button @click.prevent="sendFilesArray">отправить</button>
  </form>
</template>

<style lang="scss">
h1 {
  text-align: center;
  color: #23869b;
}
</style>
