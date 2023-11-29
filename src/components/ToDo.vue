<template>
  <div class="todo">
    <h2>Vue.js Todo List</h2>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="currentTodo" placeholder="Description" maxlength="500">
      <button class="plus-item" type="submit">+</button>
    </form>

    <div>
      <ul class="list">
        <li v-for="(todo, index) in filteredTodos" :key="index" @click="toggleTodo(todo)">
          <div class="list-grid">
            <input type="checkbox" v-model="todo.done">
            <label :class="{ 'completed': todo.done }">
              <span v-if="todo.done" class="completed">{{ todo.text }}</span>
              <span v-else>{{ todo.text }}</span>
            </label>
            <button class="del-item" @click="delTodo(todo)" >
              <img alt="Delete item" src="../assets/close-icon.png">
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
  name: 'ToDo',
    data() {
      return {
        currentTodo: '',
        todos: [
          { text: 'Learn Vue', done: false },
          { text: 'Learn Java', done: true },
          { text: 'Learn PHP', done: true },
        ],
      };
    },
    methods: {
      toggleTodo(todo) {
        todo.done = !todo.done;
        this.sortTodos();
      },
      addTodo() {
        if (!this.currentTodo.trim() || this.checkIfTodoExists()) return;
        this.todos.push({
          text: this.currentTodo,
          done: false,
        });
        this.currentTodo = '';
        this.sortTodos();
      },
      delTodo(todo) {
        this.todos = this.todos.filter((el) => el.text !== todo.text);
      },
      sortTodos() {
        this.todos.sort((a, b) => a.done - b.done);
      },
      checkIfTodoExists() {
        return this.todos.some((todo) => todo.text.toLowerCase() === this.currentTodo.trim().toLowerCase());
      },
    },
    computed: {
      filteredTodos() {
        return this.todos.filter(
          (todo) => todo.text.toLowerCase().includes(this.currentTodo.toLowerCase())
        );
      },
    },
  };
</script>

<style scoped>
.todo {
  background-color: #444448;
  color: #fff;
  border-radius: 1.15rem;
  padding: 1.75rem;
  margin: 1rem;
}
.completed {
  text-decoration: line-through;
}
h2 {
  text-align: start;
  color: #4fc08d;
  margin: 0;
  font-weight: bold;
}
form {
  display: flex;
  gap: 0.75rem;
  margin-block: 0.75rem;
}
input[type=text] {
  border-radius: 2em;
  padding: 0.3em 1em;
  border: 2px solid #4fc08d;
  width: 30%;
  background-color: transparent;
  color: #fff;
}
.plus-item {
  color: #fff;
  background-color: #4fc08d;
  border-radius: 0.25rem;
  padding: 0.4em 1em;
  border: 2px solid #4fc08d;
  transition: .2s;
  cursor: pointer;
}
.list li:nth-child(even) {
  background-color: #84A98C;
}

.list li:nth-child(odd) {
  background-color: #52796F;
}

.list {
  list-style: none;
  padding: 0;
}

.list li {
  padding: 10px;
}
.list-grid {
  display: grid;
  gap: 1rem;
  align-items: center;
  grid-template-columns: 1fr 7fr 1fr;
}
.close {
  cursor: pointer;
}
</style>
