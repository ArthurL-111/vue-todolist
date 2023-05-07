<script setup>
    import { defineProps } from "vue";
    import TodoItem from '../TodoItem/TodoItem.vue';
    const props = defineProps({
        todos: Array,
    })
</script>

<template>
    <div class="list-wrap">
        <ul>
            <TodoItem 
                v-for="todo in todos" 
                :key="todo.id"
                :title="todo.title"
                :id="todo.id"
                :isdone="todo.isdone"
            />
        </ul>
    </div>
</template>

<style scoped>
    .list-wrap{
        border: 1px solid black;
        width: 40vw;
        height: 500px;
        overflow: scroll;
    }
    ul{
        list-style: none;
        padding: 0 10px;
        display: flex;
        flex-direction: column;
        gap: 5px;
    }

    @media (max-width: 700px) {
    .list-wrap{
        width: 90vw;
    }
}
</style>