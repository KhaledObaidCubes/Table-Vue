<template>
  <div>
    {{ selectedList }}
    <Dataviewer
      v-model="selectedList"
      :requestURL="dataObject[0].url"
      :checkedRows="selectedList"
      @resolvedData="rowCreator($event)"
      @checkedRow="updateList($event)"
    >
      <template
        v-for="(name, indx) in dataObject[0].slotNames"
        :key="name"
        #[name]="{ itm }"
        ><template v-if="selectedList.indexOf(indx + 1) !== -1">
          <td class="chck-box-cell">{{ itm.name }}</td>
          <td class="chck-box-cell">{{ itm.age }}</td>
          <td class="chck-box-cell">{{ itm.id }}</td></template
        >
      </template>
    </Dataviewer>
  </div>
</template>

<script setup>
import { ref } from "vue"
import Dataviewer from "./assets/components/data-viewer/data-viewer.vue"
const selectedList = ref([11, 5, 6, 3, 7])
const updateList = ($event) => {
  console.log($event)
  if (selectedList.value.indexOf(Number($event)) == -1) {
    selectedList.value.push(Number($event))
  } else {
    selectedList.value.splice(selectedList.value.indexOf(Number($event)), 1)
  }
  console.log(selectedList.value)
}
const dataObject = ref([
  {
    url: "https://654b92025b38a59f28ef5698.mockapi.io/data",

    slotNames: [],
  },
])
//"https://run.mocky.io/v3/512a3387-8171-4578-a4ef-b01a7a3b28c4",
const rowCreator = ($event) => {
  dataObject.value[0].slotNames = $event
}
</script>

<style scoped>
.chck-box-cell {
  border-left: 1px solid #b6b4b4;
  color: red;
}
</style>
