<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
            <label for="toggle-all">Marquer les tâches comme faites</label>
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label>{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="hasTodos">
            <span class="todo-count"><strong>{{ remaining }}</strong> tâche<span v-show="remaining === 0 || remaining >= 2">s</span> à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">À faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer les tâches finies</button>
        </footer>
    </section>
</template>

<script>
    export default {
        data () {
            return {
                todos: [{
                    name: 'Tâche de test',
                    completed: false
                }],
                newTodo: '',
                filter: 'all'
            }
        },
        methods: {
            addTodo () {
                this.todos.push({
                    completed: false,
                    name: this.newTodo
                })
                this.newTodo = ''
            },
            deleteTodo (todo) {
                this.todos = this.todos.filter(i => i !== todo)
            },
            deleteCompleted () {
                this.todos = this.todos.filter(todo => !todo.completed)
            }
        },
        computed: {
            allDone: {
                get () {
                    return this.remaining === 0
                },
                set (value) {
                    this.todos.forEach(todo => {
                        todo.completed = value
                    })
                }
            },
            remaining () {
                return this.todos.filter(todo => !todo.completed).length
            },
            completed () {
                return this.todos.filter(todo => !todo.completed).length
            },
            hasTodos () {
                return this.todos.length > 0
            },
            filteredTodos () {
                if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed)
                }
                return this.todos
            }
        }
    }
</script>

<style src="./todos.css"></style>