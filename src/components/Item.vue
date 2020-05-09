<template>
    <div class="item" :class="{'completed': todo.completed}">
        <div class="item-check">
            <label class="container" for="completed"></label>
            <input class="checkmark" id="completed" type="checkbox"
                   :checked="todo.completed"
                   v-on:change="toggleCompleted"
            >
        </div>
        <div class="item-data">
            <div v-if="!this.edit">
                <h3>{{ todo.title }}</h3>
                <p>{{ todo.description }}</p>
            </div>
            <div v-else>
                <div>
                    <label for="title-input"></label>
                    <input
                            id="title-input"
                            class="item-field"
                            placeholder="Title"
                            v-model="local_title"
                    >
                </div>
                <div>
                    <label for="title-description"></label>
                    <textarea
                            id="title-description"
                            class="item-field"
                            placeholder="Description"
                            v-model="local_description"
                            @input="resize"
                    >
                    </textarea>
                </div>
            </div>
        </div>
        <div class="item-actions">
            <button v-if="!this.edit" class="btn" @click="editTodo">edit</button>
            <button v-else class="btn" @click="saveTodo">save</button>

            <button v-if="!this.edit" class="btn" @click="deleteTodo">delete</button>
            <button v-else class="btn" @click="resetDelete">cancel</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Item",
        props: ["todo"],
        data() {
            return {
                edit: false,
                local_title: this.todo.title,
                local_description: this.todo.description
            }
        },
        methods: {
            deleteTodo(event) {
                event.preventDefault();
                this.$emit('delete-item', this.todo.id);
            },
            editTodo(event) {
                event.preventDefault();
                this.edit = !this.edit;
            },
            toggleCompleted(event) {
                event.preventDefault();
                this.todo.completed = !this.todo.completed;
            },
            resetDelete(event = null) {
                if (event) {
                    event.preventDefault();
                }
                this.edit = false;
            },
            saveTodo(event) {
                event.preventDefault();

                this.todo.title = this.local_title;
                this.todo.description = this.local_description;

                this.$emit('update-item', this.todo);

                this.resetDelete();
            },
            resize() {
                var tx = document.getElementsByTagName('textarea');
                for (var i = 0; i < tx.length; i++) {
                    tx[i].setAttribute('style', 'height:' + (tx[i].scrollHeight) + 'px;overflow-y:hidden;');
                    tx[i].addEventListener("input", OnInput, false);
                }

                function OnInput() {
                    this.style.height = 'auto';
                    this.style.height = (this.scrollHeight) + 'px';
                }
            }
        }
    }
</script>

<style scoped>
    .completed {
        text-decoration: line-through;
        background-color: darkgray !important;
        filter: brightness(50%);
        color: black !important;
    }

    .item {
        background-color: black;
        border: 1px solid lawngreen;
        margin: 2em;
        border-radius: 1em;
        color: white;

        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: space-around;
    }

    .item-actions .btn {
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

    .item-field {
        width: 500px;
        background-color: transparent;
        color: white;
        border: white 1px solid;
        padding: 1em;
    }

    textarea.item-field {
        margin: 1em 0 2em 0;
    }

    input.item-field {
        margin: 2em 0 1em 0;
    }

</style>