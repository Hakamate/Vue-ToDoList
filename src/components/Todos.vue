<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" v-model="newTodo" @keyup.enter="addTodo"
                placeholder="Ajouter une tâche">
        </header>
        <div class="main">
            <input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone">
            <label for="toggle-all"></label>
            <ul class="todo-list">
                <!-- v-bind to use v-for -->
                <li class="todo" :class="{completed: todo.completed, editing: todo === editing}" v-bind:key="todo.id"
                    v-for="todo in filteredTodos">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label @dblclick="editTodo(todo)">{{todo.name}}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name"  @blur="doneEdit" @keyup.esc="cancelEdit" @keyup.enter="doneEdit" v-focus="todo === editing">
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="hasTodos">
            <span class="todo-count"> <strong>{{ remaining }}</strong> Tâche(s) à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">All</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">Todo</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Task(s) ended</a>
                </li>
            </ul>
            <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Delete done Task(s)</button>
        </footer>
    </section>

</template>

<script>
    // TODO: some import
import Vue from 'vue'
    export default {
        // Declare state
        data() {
            // return an object
            return {
                todos: [

                    ],
                // save new task
                newTodo: '',

                //filter state
                filter: 'all',

                // task that we change
                editing: null,

                oldTodo: ""
            }
        },
        methods: {
            addTodo() {
                this.todos.push({
                    completed: false,
                    name: this.newTodo,
                })
                // Empty input
                this.newTodo = ''
            },
            deleteTodo(todo){
                // Keep all todo but not the selected todo
                this.todos = this.todos.filter(i => i !== todo)
            },
            deleteCompleted(){
                this.todos = this.todos.filter(todo => !todo.completed)
            },
            editTodo(todo){
                this.editing = todo
                this.oldTodo = todo.name
            },
            doneEdit(){
                this.editing = null
            },
            cancelEdit(){
                this.editing.name = this.oldTodo
                this.doneEdit()
            }
        },
        computed: {
            allDone: {
                get() {
                    return this.remaining === 0
                },
                set(value) {
                   this.todos.forEach(todo => {
                       todo.completed = value
                   })
                }
            },
            remaining() {
                // Take todo list and use filter to obtain remaining
                // We just keep todo witch they're completed
                return this.todos.filter(todo => !todo.completed).length
            },
            completed() {
                // Take todo list and use filter to obtain remaining
                // We just keep todo witch they're completed
                return this.todos.filter(todo => todo.completed).length
            },
            hasTodos(){
                return this.todos.length > 0
            },
            filteredTodos() {
                if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed)
                }

                return this.todos


            }
        },

        directives: {
            focus(el, value) {
                if(value){
                    Vue.nextTick(_ => {
                        el.focus()
                    })
                }
            }
        }

    }
</script>

<style src="../todos.css">

</style>