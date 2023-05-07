<script setup>
    import { defineProps, inject, ref } from "vue";

    const props = defineProps({
        title: String,
        id: Number,
        isdone: Boolean,
    });

    const editing = ref(false);
    const titel_val = ref(props.title)

    const { deleteTodo, doneTodo, editTodo } = inject('todos_modify')

    const toggleEditing = () => {
        console.log('toggle')
        editing.value = !editing.value;
    }

    const saveEdit = (id, newTitle) => {
        console.log('save')
        if (newTitle.trim() !== "") {
            editTodo(id, newTitle);
        }
        toggleEditing()
    }
</script>

<template>
    <li>
        <div v-if="!editing">{{ title }}</div>     
        <input 
            v-else 
            type="text"
            v-model="titel_val"
            @keyup.enter="() => saveEdit(id, titel_val)"
        >
        <button v-if="!editing" @click="toggleEditing">Edit</button>
        <button v-else @click="() => saveEdit(id, titel_val)">Save</button>
        <button @click="() => deleteTodo(id)">Delete</button>
        <button @click="() => doneTodo(id, isdone)">{{ isdone ? 'Undo' : 'Done' }}</button>
    </li>
</template>

<style scoped>
li {
    display: flex;
    flex-direction: row;
    gap: 5px;
}

li button {
    flex: 0 0 50px;
}

li div {
    flex: auto;
    overflow: hidden;
    text-overflow: ellipsis;
}

li input {
    flex: auto;
}
</style>