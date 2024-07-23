<script setup>
//【インポート】
import { statuses } from '@/const/statuses';
import { ref } from 'vue';

//【変数定義】
let items = ref(JSON.parse(localStorage.getItem("items")) || []);
let inputContent = ref();//タスクの内容
let inputLimit = ref();//タスクの期限
let inputState = ref();//タスクの状態
let errMsg = ref();//エラーメッセージの内容を管理
let isErrMsg = ref(false);//エラーメッセージの表示を管理
let isShowModal = ref(false);//モーダルの表示を管理
let deleteItemId = ref();//削除するタスクのidを管理
let deleteItemContent = ref();//削除するタスクの内容を管理


//【メソッド定義】
const onEdit = (id) => {
    //他に編集モードのタスクが無いか調べる
    let isOnEditOther = false;

    items.value.map((item) => {
        if (item.onEdit) {
            isOnEditOther = true;
            return;
        }
    })

    if (isOnEditOther) {
        errMsg.value = "他に編集中のタスクがあります";
        isErrMsg.value = true;
        return;
    } else {
        inputContent.value = items.value[id].content;
        inputLimit.value = items.value[id].limit;
        inputState.value = items.value[id].state;
        items.value[id].onEdit = true;
    }
}

const onUpdate = (id) => {
    if (inputContent.value == "" || inputLimit.value == "") {
        errMsg.value = "タスク・期限を両方入力してください"
        isErrMsg.value = true;
        return;
    }

    //タスクの上書き保存をする
    //入力された値を使って、新しいタスクのオブジェクトを作る
    const newItem = {
        id: id,
        content: inputContent.value,
        limit: inputLimit.value,
        state: inputState.value,
        onEdit: false,
    }

    items.value.splice(id, 1, newItem);
    localStorage.setItem("items", JSON.stringify(items.value));
    isErrMsg.value = false;
}

const showDeleteModal = (id) => {
    isShowModal.value = true;
    deleteItemId = id;
    deleteItemContent = items.value[id].content
}

const onCloseModal = () => {
    isShowModal.value = false;
}

const onDeleteItem = () => {
    items.value.splice(deleteItemId, 1);

    items.value = items.value.map((item, index) => ({
        id: index,
        content: item.content,
        limit: item.limit,
        state: item.state,
        onEdit: item.onEdit,
    }));

    localStorage.setItem("items", JSON.stringify(items.value));

    isShowModal.value = false;
}
</script>

<template>
    <div>
        <p class="c-red mgT10" v-if="isErrMsg">{{ errMsg }}</p>
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
                        <option v-for="state in statuses" :key="state.id" :value="state"
                            :selected="state.id == item.state.id">
                            {{ state.value }}
                        </option>
                    </select>
                </td>
                <td>
                    <button v-if="!item.onEdit" @click="onEdit(item.id)">編集</button>
                    <button v-else @click="onUpdate(item.id)">完了</button>
                </td>
                <td><button @click="showDeleteModal(item.id)">削除</button></td>
            </tr>
        </table>

        <div v-if="isShowModal" class="modal">
            <div class="modal-content">
                <p>「{{ deleteItemContent }}」を削除してもよろしいですか？</p>
                <button @click="onDeleteItem()">はい</button>
                <button @click="onCloseModal()">キャンセル</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.c-red {
    color: red;
}

.mgT10 {
    margin-top: 10px;
}

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


.modal {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    z-index: 1000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    text-align: center;
}

.modal-content button {
    margin: 10px;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.modal-content button:first-of-type {
    background-color: #4CAF50;
    color: white;
}

.modal-content button:last-of-type {
    background-color: #f44336;
    color: white;
}
</style>
