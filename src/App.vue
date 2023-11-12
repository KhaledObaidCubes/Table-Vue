<template>
  <div>
    <Dataviewer
      v-for="(itm, idx) in dataObject"
      :key="itm"
      :requestURL="itm.url"
      @resolvedData="columnCreator(idx, $event)"
    >
      <ColumnViewer
        v-for="key in itm.dataKeys"
        :key="key"
        :singleColumn="[key, itm.dataObject]"
      />
    </Dataviewer>
  </div>
</template>

<script setup>
import { ref } from "vue"
import Dataviewer from "./assets/components/data-viewer/data-viewer.vue"
import ColumnViewer from "./assets/components/data-viewer/column-viewer.vue"
const dataObject = ref([
  {
    url: "https://654b92025b38a59f28ef5698.mockapi.io/data",
    dataKeys: [],
    dataObject: [],
  },
  {
    url: "https://run.mocky.io/v3/512a3387-8171-4578-a4ef-b01a7a3b28c4",
    dataKeys: [],
    dataObject: [],
  },
])
const columnCreator = (indx, $event) => {
  console.log($event[0])
  dataObject.value[indx].dataObject = $event[0]
  dataObject.value[indx].dataKeys = $event[1]
}
</script>

<style scoped></style>
