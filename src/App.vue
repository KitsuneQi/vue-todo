<template>
    <div class="app">
        <h1> 我的第一个TODO</h1>
        <!--输入框+ 添加按钮-->
        <div class="input-row">
            <input v-model="newTodo" @keyup.enter="输入任务, 按回车添加" />
            <button @click="addTodo">添加</button>
        </div>

        <!--任务列表-->
        <ul class="todo-list">
            <li v-for="(todo, imdex) in todos" :key="todo.id">
                <input type="checkbox" v-model="todo.done" />
                <span :class="{ done: todo.done }">{{ todo.text }}</span>
                <button @click="removeTodo(index)">删除</button>
            </li>
        </ul>

        <!--统计-->
        <p>共{{ todos.length }}项，已完成{{ doneCount }}项</p>
    </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

//从local Storage读取，就没有空数组
const todos = ref(JSON.parse(localStorage.getItem('todos') || '[]'))
const newTodo = ref('')

//添加
function addTodo() {
    const text = newTodo.value.trim()
    if (!text) return
    todos.value.push({
        id: Date.now(),
        text,
        done: false
    })
    newTodo.value = ''
}

// 删除
function removeTodo(index) {
    todos.value.splice(index, 1)
}

// 统计已完成
const doneCount = computed(() => {
    return todos.value.filter(t => t.done).length
})

// 自动保存到 localStorage
watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })
</script>

<style>
.app {
    max-width: 500px;
    margin: 40px auto;
    font-family: sans-serif;
}

.input-row {
    display: flex;
    gap: 8px;
    margin-bottom: 16px;
}

input[type="text"],
input:not([type]) {
    flex: 1;
    padding: 8px;
}

.todo-list {
    list-style: none;
    padding: 0;
}

.todo-list li {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 8px 0;
    border-bottom: 1px solid #eee;
}

.todo-list li span {
    flex: 1;
}

.done {
    text-decoration: line-through;
    color: #999;
}

button {
    padding: 6px 12px;
    cursor: pointer;
}
</style>