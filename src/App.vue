<template>
    <div id="app">
        <Todos v-bind:todos="todos"
               v-on:delete-item="deleteItem"
               v-on:update-item="updateItem"
        />
    </div>
</template>

<script>

    import Todos from "./components/Todos";

    export default {
        name: 'App',
        components: {
            Todos
        },
        data() {
            return {
                todos: []
            }
        },
        methods: {
            deleteItem(id) {
                this.todos = this.todos.filter(element => element.id !== id);
            },
            updateItem(todo) {
                this.todos.forEach((element, index) => {
                    if (element.id === todo.id) {
                        this.todos[index] = todo;
                    }
                });
            }
        }
    }
</script>

<style>
    #app {
        background-color: black;
        color: white;
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        padding-bottom: 100px;
        padding-top: 100px;
    }
</style>
