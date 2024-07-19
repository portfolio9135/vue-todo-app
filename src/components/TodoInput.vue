<script setup>
import { ref } from 'vue';

import { statuses } from "../const/statuses";

const input = ref("");
const inputDate = ref("");

const onSubmitForm = () => {
  // localStorageに保存されているデータは文字列形式(JSON形式)なので、JSON.parseで配列に変換する
  const items = JSON.parse(localStorage.getItem("items")) || [];
  const newItem = {
    id: items.length,
    content: input.value,
    limit: inputDate.value,
    state: statuses.NOT_START,
    onEdit: false,
  };
  items.push(newItem);
  //配列のままでは保存できないので、JSON.stringifyで文字列形式(JSON形式)に変換してからlocalStorageに保存する
  localStorage.setItem("items", JSON.stringify(items));
}
</script>

<template>
  <div>
    <form @submit="onSubmitForm">
      <label>やること<input type="text" v-model="input" /></label>
      <label>期限<input type="date" v-model="inputDate"/></label>
      <input type="submit" value="登録！" />
    </form>
    
    <div>
      <p>{{ input }}</p>
    </div>
  </div>
</template>