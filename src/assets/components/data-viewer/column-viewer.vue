<template>
  <div class="column-container">
    <div class="table-wrapper">
      <div class="table-scroll">
        <table>
          <th>{{ singleColumn[0] }}</th>
          <tr v-for="(key, val, idx) in singleColumn[1]" :key="val">
            <td>{{ key[singleColumn[0]] }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue"
defineProps(["singleColumn"])
</script>
<style scoped>
table {
  text-align: center;
}
table th {
  color: azure;
  background-color: #5a5656;
  width: 200px;
  float: left;
}
.column-container {
  float: left;
  margin-top: 10px;
}
table td {
  border: 1px solid black;
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
</style>
