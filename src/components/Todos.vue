<template>
    <AddTodo 
        @add-todo="addTodo"
    />
    <TodoItem 
        v-for="todo in todos"
        :key="todo.id"
        :todoProps="todo"
        @item-completed="markComplete"
        @delete-item="deleteTodo"
    />
</template>

<script>
//import { v4 as uuidv4 } from 'uuid'
import axios from 'axios'
import { ref } from 'vue'
import TodoItem from './TodoItem';
import AddTodo from './AddTodo';
export default {
    name: 'Todos',
    components: {TodoItem, AddTodo},
    setup(){
        // const todos = ref([
            // {
            //     id: uuidv4,
            //     title: 'Viec 1',
            //     completed: false
            // },
            // {
            //     id: uuidv4,
            //     title: 'Viec 2',
            //     completed: true
            // },
            // {
            //     id: uuidv4,
            //     title: 'Viec 3',
            //     completed: false
            // },
        // ])
        const todos = ref([])
        const getAllTodos = async () => {
            try {
                const res = await axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
                todos.value = res.data
            } catch (error) {
                console.log(error);
            }
        } 
        getAllTodos()
        //change du lieu
        const markComplete = id => {
            todos.value = todos.value.map(todo => {
                if(todo.id === id) todo.completed = !todo.completed
                return todo
            })
        }
        //xoa du lieu
        const deleteTodo = async id => {
            try {
                await axios.delete('https://jsonplaceholder.typicode.com/todos/${id}')
                todos.value = todos.value.filter(todo => todo.id !== id)
            } catch (error) {
                console.log(error);
            }
        }

        const addTodo = async newTodo => {
            try {
                const res = await axios.post('https://jsonplaceholder.typicode.com/todos',newTodo)
                todos.value.push(res.data)
            } catch (error) {
                console.log(error);
            }
            // console.log(newTodo.id);
            // todos.value.push(newTodo)
        }
        return {
            todos,
            markComplete,
            deleteTodo,
            addTodo
        }
    }
}
</script>

<style scoped>

</style>