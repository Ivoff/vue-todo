<template>
    <div>
        <h1>Todos</h1>

        <button
                v-if="!create"
                @click="toggleCreate"
                class="btn"
        >
            add item
        </button>
        <Add
                v-else
                :create="create"
                v-on:create-cancel="cond => create = cond"
                v-on:create-add="addCreate"
        />

        <div v-for="todo in todos" v-bind:key="todo.id">
            <Item
                    :todo="todo"
                    v-on:delete-item="$emit('delete-item', todo.id)"
                    v-on:update-item="$emit('update-item', todo)"
            />
        </div>
    </div>
</template>

<script>

    import Item from "@/components/Item";
    import Add from "@/components/Add";

    export default {
        name: "Todos",
        components: {
            Item,
            Add
        },
        props: ["todos"],
        data() {
            return {
                create: false
            }
        },
        methods: {
            toggleCreate(event) {
                event.preventDefault();
                this.create = !this.create;
            },
            addCreate(todo) {
                todo["completed"] = false;
                todo["id"] = this.todos.length + 1;
                this.todos.push(todo);
            }
        }
    }
</script>

<style scoped>
    .btn {
        padding: .6em;
        margin: 0 .5em;
        background-color: transparent;
        color: white;
        border-radius: .5em;
        border: lawngreen 1px solid;
    }

    .btn:hover {
        background-color: white;
        color: black;
    }
</style>