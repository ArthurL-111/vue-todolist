<script setup>

    import { ref, onMounted, provide } from 'vue';
    import axios from "axios";
    import ListWrap from './ListWrap.vue';

    const todo_title = ref('');
    const todos = ref([]);

    async function addTodo () {
        if (todo_title.value.trim() === "") {
            alert("Please input something!");
            return
        }
        const new_todo = {
            "title": todo_title.value,
            "isdone": false,
        }
        const reponse = await axios.post('http://localhost:3000/todos', new_todo);
        todos.value.push(reponse.data);
        console.log(todos.value);
    }

    async function deleteTodo (id) {
        await axios.delete(`http://localhost:3000/todos/${id}`);
        todos.value = todos.value.filter(todo => todo.id !== id);
    }

    async function doneTodo (id, isdone) {
        await axios.patch(`http://localhost:3000/todos/${id}`,{
            isdone: !isdone
        });
        todos.value = todos.value.map(todo => {
            if (todo.id !== id) {
                return todo;
            } else {
                return {...todo, isdone: !todo.isdone}
            }
        })
    }

    async function editTodo (id, title) {
        await axios.patch(`http://localhost:3000/todos/${id}`,{
            title: title
        });
        todos.value = todos.value.map(todo => {
            if (todo.id !== id) {
                return todo;
            } else {
                return {...todo, title: title}
            }
        })
    }

    onMounted( async () => {
        const reponse = await axios.get('http://localhost:3000/todos');
        todos.value = reponse.data;
        console.log(todos.value)
    });

    provide('todos_modify',{
        deleteTodo,
        doneTodo,
        editTodo
    })

</script>

<template>

    <div class="input-warp">
        <input type="text" v-model="todo_title">
        <button @click="addTodo">Add</button>
    </div>

    <div class="lists">
        <ListWrap 
            :todos="todos.filter(todo => todo.isdone === false).reverse()"
        />
        <ListWrap 
            :todos="todos.filter(todo => todo.isdone === true).reverse()"
        />
    </div>
    
</template>

<style scoped>
.lists{
    display: flex;
    flex-direction: row;
    margin-top: 30px;
    gap: 20px
}

@media (max-width: 700px) {
    .lists{
        flex-direction: column;
    }
}

</style>