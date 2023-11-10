<template>
  <div>
    <Dataviewer :requestURL="URL" />
    <Dataviewer :requestURL="URL2" />
  </div>
</template>

<script setup>
import { ref } from "vue"
import Dataviewer from "./assets/components/data-viewer/data-viewer.vue"
const URL = ref("https://654b92025b38a59f28ef5698.mockapi.io/data")
const URL2 = ref("https://run.mocky.io/v3/512a3387-8171-4578-a4ef-b01a7a3b28c4")
</script>

<style scoped></style>
