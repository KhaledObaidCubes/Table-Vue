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
const URL2 = ref(
  "https://api.json-generator.com/templates/smkmiq7L320t/data?access_token=8zj9pta04l3glhsrp6lvlvrlit6w6iejgwlq3s9e"
)
</script>

<style scoped></style>
