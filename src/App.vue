<template>
  <div>
    <Dataviewer
      :data="filteredData"
      :filters="filters"
      @filterString="updateSearchStr($event)"
      @categoryUpdate="updateCategory($event)"
      @ascort="ascortData($event)"
      @descort="descortData($event)"
    />
  </div>
</template>

<script setup>
import { ref, computed } from "vue"
const data = ref([])
const filters = ref(["name", "id", "age"])
import Dataviewer from "./assets/components/data-viewer/data-viewer.vue"
fetch("https://654b92025b38a59f28ef5698.mockapi.io/data")
  .then((r) => r.json())
  .then((r) => {
    data.value = r
  })
  .catch(() => {
    console.error("Responce Error")
  })
const fltr = ref("name")
const fltrSTR = ref("")
const updateSearchStr = (strVal) => {
  fltrSTR.value = strVal
}
const updateCategory = (searchCategory) => {
  fltr.value = searchCategory
  console.log(searchCategory)
}
const ascortData = (sortBaseCategory) => {
  const compare = (a, b) => {
    if (a[sortBaseCategory] < b[sortBaseCategory]) {
      return -1
    }
    if (a[sortBaseCategory] > b[sortBaseCategory]) {
      return 1
    }
    return 0
  }

  filteredData.value.sort(compare)
}

const descortData = (sortBaseCategory) => {
  const compare = (b, a) => {
    if (a[sortBaseCategory] < b[sortBaseCategory]) {
      return -1
    }
    if (a[sortBaseCategory] > b[sortBaseCategory]) {
      return 1
    }
    return 0
  }

  filteredData.value.sort(compare)
}

const filteredData = computed(() => {
  if (fltrSTR.value.length) {
    const result = ref([])
    data.value.forEach((element) => {
      for (const [key, value] of Object.entries(element)) {
        if (
          key == fltr.value &&
          String(value).indexOf(String(fltrSTR.value)) != -1
        ) {
          result.value.push(element)
        }
      }
    })
    return result.value
  }
  return data.value
})
</script>

<style scoped></style>
