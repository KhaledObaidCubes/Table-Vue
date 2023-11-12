<template>
  <div v-if="!data.length" class="loader-spenner">
    <img src="../../images/loading.gif" alt="" />
  </div>
  <div v-else>
    <div>
      <div class="search-section">
        <label for="search" class="label">Search category:</label>
        <select
          name="search"
          id="search"
          class="select-filter"
          v-model="selectCategory"
          @change="categoryUpdate(selectCategory)"
        >
          <option disabled value="">select search key</option>
          <option v-for="filter in filters" :key="filter" :value="filter">
            {{ filter }}
          </option>
        </select>
      </div>
      <div class="search-section">
        <input
          type="text"
          class="search-input"
          v-model="inputValue"
          @keyup="filterString(inputValue)"
        />
      </div>
    </div>
    <div class="table-wrapper">
      <div class="table-scroll">
        <table class="data-view">
          <tr class="info-title">
            <th class="chck-box-cell"></th>
            <th v-for="(value, key) in filteredData[0]" :key="key">
              {{ key }}
              <div class="sort-options">
                <img src="../../images/ascending.png" @click="descort(key)" />
                <img src="../../images/descending.png" @click="ascort(key)" />
              </div>
            </th>
          </tr>
          <tr v-for="itm in filteredData" :key="itm">
            <td
              v-if="props.checkedRows.indexOf(Number(itm.id)) > -1"
              class="chck-box-cell"
            >
              <input
                type="checkbox"
                @change="emitsData('checkedRow', itm.id)"
                :name="`column-${itm.id}`"
                checked
              />
            </td>
            <td v-else class="chck-box-cell">
              <input
                type="checkbox"
                @change="emitsData('checkedRow', itm.id)"
                :name="`column-${itm.id}`"
              />
            </td>
            <slot :name="`column-${itm.id}`" :itm="itm">
              <td v-for="([k, v], key) in Object.entries(itm)" :key="key">
                {{ v }}
              </td>
            </slot>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, computed, ref, defineEmits } from "vue"
const props = defineProps(["requestURL", "checkedRows"])
const inputValue = ref("")
const selectCategory = ref("")
const data = ref([])
const filters = ref([])
const slotsNames = ref([])
const emitsData = defineEmits(["resolvedData", "checkedRow"])
async function fetchData(req) {
  try {
    const response = await fetch(req)
    const dataValue = await response.json()

    data.value = dataValue
    filters.value = Object.keys(data.value[0])
    for (let id in Object.keys(data.value)) {
      slotsNames.value.push(`column-${Number(id) + 1}`)
    }
    emitsData("resolvedData", slotsNames.value)
  } catch (error) {
    console.error("Response Error", error)
  }
}

// Call the async function
fetchData(props.requestURL)
const fltr = ref("name")
const fltrSTR = ref("")
const filterString = (strVal) => {
  fltrSTR.value = strVal
}
const categoryUpdate = (searchCategory) => {
  fltr.value = searchCategory
}
const ascort = (sortBaseCategory) => {
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

const descort = (sortBaseCategory) => {
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

<style scoped>
.data-view {
  border-collapse: collapse;
}
.data-view tr,
.data-view td {
  width: 150px;
  height: 30px;
  border: 1px solid #b6b4b4;
  border-collapse: collapse;
  padding: 0;
  text-align: center;
  font-family: Tahoma;
}
.label {
  font-family: Tahoma;
}
.info-title {
  color: white;
  background-color: #1e3050;
}
.table-wrapper {
  position: relative;
}
.table-scroll {
  height: 200px;
  overflow: auto;
  margin-top: 20px;
  border: 1px solid black;
}
.table-wrapper table {
  width: 100%;
}
.search-section {
  margin-top: 5px;
  width: 300px;
}
.search-input {
  width: 100%;
}
.select-filter {
  margin-left: 10px;
  width: 170px;
}
.sort-options {
  width: 15px;
  clear: both;
  float: right;
}
.sort-options img {
  width: 15px;
  float: right;
  height: 8px;
  margin-top: 3px;
}
.loader-spenner {
  width: 100%;
  border: 1px solid black;
  text-align: center;
}
.loader-spenner img {
  width: 120px;
}
.default-slot {
  float: left;
}
.chck-box-cell {
  width: 30px !important;
}
</style>
