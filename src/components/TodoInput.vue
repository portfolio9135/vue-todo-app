<script setup>
import { ref } from 'vue';

import { statuses } from "../const/statuses";

const input = ref("");
const inputDate = ref("");
const isErrMsg = ref(false);

const onSubmitForm = (e) => {
  if (input.value == "" || inputDate.value == "") {
    e.preventDefault();
    isErrMsg.value = true;
    return;
  }

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
    <p class="c-red" v-if="isErrMsg">タスク・期限を両方入力してください</p>
    <form @submit="onSubmitForm">
      <label>やること<input type="text" v-model="input" /></label>
      <label>期限<input type="date" v-model="inputDate" /></label>
      <input type="submit" value="登録！" />
    </form>

    <div>
    </div>
  </div>
</template>

<style scoped>
.c-red {
  color: red;
}
</style>