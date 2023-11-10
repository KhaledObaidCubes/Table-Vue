<template>
  <div>
    <Dataviewer :requestURL="requestURL" />
  </div>
</template>

<script setup>
import { ref } from "vue"
const requestURL = ref("https://654b92025b38a59f28ef5698.mockapi.io/data")
import Dataviewer from "./assets/components/data-viewer/data-viewer.vue"
</script>
