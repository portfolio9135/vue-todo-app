<script setup>
import { statuses } from '@/const/statuses';
import { ref } from 'vue';

let items = ref( JSON.parse(localStorage.getItem("items")) || []);
let inputContent = ref();//タスクの内容
let inputLimit = ref();//タスクの期限
let inputState = ref();//タスクの状態

const onEdit = (id) => {
    inputContent.value = items.value[id].content;
    inputLimit.value = items.value[id].limit;
    inputState.value = items.value[id].state;
    items.value[id].onEdit = true;
}
</script>

<template>
    <div>
        <table>
            <tr>
                <th class="th-id">ID</th>
                <th class="th-value">やること</th>
                <th class="th-limit">期限</th>
                <th class="th-state">状態</th>
                <th class="th-edit">編集</th>
                <th class="th-delete">削除</th>
            </tr>
            <tr v-for="item in items" :key="item.id">
                <td>{{ item.id }}</td>
                <td>
                    <span v-if="!item.onEdit">{{ item.content }}</span>
                    <input v-else type="text" v-model="inputContent">
                </td>
                <td>
                    <span v-if="!item.onEdit">{{ item.limit }}</span>
                    <input v-else type="date" v-model="inputLimit">
                </td>
                <td>
                    <span v-if="!item.onEdit">{{ item.state.value }}</span>
                    <select v-else v-model="inputState">
                        <option
                          v-for="state in statuses" 
                          :key="state.id"
                          :value="state"
                          :selected="state.id == item.state.id"
                        >
                            {{ state.value }}
                        </option>
                    </select>
                </td>
                <td><button @click="onEdit(item.id)">編集</button></td>
                <td><button>削除</button></td>
            </tr>
        </table>
    </div>
</template>

<style scoped>
table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
    font-size: 1em;
    text-align: left;
    background-color: #f2f2f2;
}

th,
td {
    padding: 12px 15px;
    border: 1px solid #ddd;
}

th {
    background-color: #009879;
    color: #ffffff;
    font-weight: bold;
}

tr:nth-of-type(even) {
    background-color: #f3f3f3;
}

tr:hover {
    background-color: #f1f1f1;
}

.th-id {
    width: 5%;
}

.th-value {
    width: 35%;
}

.th-limit {
    width: 20%;
}

.th-state {
    width: 10%;
}

.th-edit,
.th-delete {
    width: 15%;
    text-align: center;
}

th,
td {
    text-align: center;
}

button {
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button.edit {
    background-color: #4CAF50;
    color: white;
}

button.delete {
    background-color: #f44336;
    color: white;
}
</style>
