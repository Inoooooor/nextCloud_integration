<script setup>
import HDE from '../plugin'
import { ref, watch } from 'vue'

const reader = new FileReader()

const fileToUpload = ref(null)

// const logFile = (event) => console.log(event.target.files)

const updateFile = (event) => {
  fileToUpload.value = event.target.files[0]
  reader.readAsArrayBuffer(fileToUpload.value)

  console.log('file ->', document.querySelector('#file'))
}

reader.onload = async () => {
  const arrayBuffer = reader.result

  const options = {
    url: 'https://data.tinkoff.ru/remote.php/dav/files/HDE_cloud/testDir/testUploadForm.txt',
    auth: 'hdeCloud',
    method: 'PUT',
    contentType: 'application/octet-stream',
    headers: {
      'Content-Length': arrayBuffer.byteLength.toString(),
    },
    data: arrayBuffer,
  }
  try {
    const { data } = await HDE.request(options)
    console.log(data)
  } catch (error) {
    console.log(error)
  }
}

reader.onerror = (error) => {
  console.error(error)
}

watch(fileToUpload, (newValue) => console.log(newValue))
</script>

<template>
  <form id="cloudForm">
    <label for="file">В облако </label>
    <input id="file" type="file" name="file" @change="updateFile($event)" />
    <button @click.stop="uploadFile()">отправить</button>
  </form>
</template>

<style lang="scss">
h1 {
  text-align: center;
  color: #23869b;
}
</style>
