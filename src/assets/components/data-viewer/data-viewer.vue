<template>
  <div>
    <div class="search-section">
      <label for="search" class="label">Search category:</label>
      <select
        name="search"
        id="search"
        class="select-filter"
        v-model="selectCategory"
        @change="dispatcher('categoryUpdate', selectCategory)"
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
        @keyup="dispatcher('filterString', inputValue)"
      />
    </div>
  </div>
  <div class="table-wrapper">
    <div class="table-scroll">
      <table class="data-view">
        <tr class="info-title">
          <th v-for="(value, key) in data[0]" :key="key">
            {{ key }}
            <div class="sort-options">
              <img
                src="../../images/ascending.png"
                @click="dispatcher('descort', key)"
              />
              <img
                src="../../images/descending.png"
                @click="dispatcher('ascort', key)"
              />
            </div>
          </th>
        </tr>
        <tr v-for="itm in data" :key="itm">
          <td v-for="(info, pro, idx) in itm" :key="idx">
            {{ info }}
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref } from "vue"
defineProps(["data", "filters"])
const dispatcher = defineEmits([
  "filterString",
  "categoryUpdate",
  "ascort",
  "descort",
])

const inputValue = ref("")
const selectCategory = ref("")
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
}
.table-wrapper table {
  width: 100%;
}
.search-section {
  margin-top: 5px;
  width: 200px;
}
.search-input {
  width: 100%;
}
.select-filter {
  margin-left: 10px;
  width: 69px;
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
</style>
