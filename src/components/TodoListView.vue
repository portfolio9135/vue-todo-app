<script setup>
const items = JSON.parse(localStorage.getItem("items")) || [];
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
                <td>{{ item.content }}</td>
                <td>{{ item.limit }}</td>
                <td>{{ item.state.value }}</td>
                <td><button>編集</button></td>
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

th, td {
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

.th-edit, .th-delete {
    width: 15%;
    text-align: center;
}

th, td {
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
