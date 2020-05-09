<template>
    <div class="create">
        <label for="create-input"></label>
        <input
                id="create-input"
                type="text"
                class="text-field"
                placeholder="title"
                v-model="title"
        >

        <label for="create-textarea"></label>
        <textarea
                id="create-textarea"
                class="text-field"
                placeholder="description"
                @input="resize"
                v-model="description"
        >
        </textarea>

        <div class="create-actions">
            <button
                    class="btn"
                    @click="addItem"
            >
                add
            </button>
            <button
                    class="btn"
                    @click="$emit('create-cancel', false)"
            >
                cancel
            </button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Add",
        props: ["create"],
        data() {
            return {
                title: "",
                description: ""
            }
        },
        methods: {
            addItem(event) {
                event.preventDefault();
                this.$emit('create-add', {
                    title: this.title,
                    description: this.description
                });
                this.$emit('create-cancel', false);
            },
            resize(){
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
    .create {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .text-field {
        padding: 1em;
        background-color: transparent;
        color: white;
        border: white 1px solid;
        width: 50vw;
    }

    input.text-field {
        margin-bottom: 1.5em;
    }

    .btn {
        background-color: transparent;
        padding: .5em 1em;
        color: white;
        border: white 1px solid;
    }

    .create-actions {
        margin-top: 1em;
    }
</style>