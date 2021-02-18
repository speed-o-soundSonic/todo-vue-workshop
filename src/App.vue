<template>
  <h1 class="text-center">{{ title }}</h1>
  <div class="container">
    <form @submit.prevent="addTodo">
      <div class="form-group">
        <label for="to-do">What do you want to do?</label
        ><input
          v-model="task"
          type="text"
          id="to-do"
          class="form-control"
          placeholder="Enter task"
        /><small id="emailHelp" class="form-text text-muted"
          >You actually need to do it 🙃</small
        >
      </div>
    </form>
    <div class="red">
      <ul v-for="todo in todos" :key="todo.id">
        <li class="d-flex align-items-center mt-4">
          <input type="checkbox" v-model="todo.done" class="mx-4" />
          <input
            v-if="todo.edit"
            type="text"
            id="edit-to-do"
            class="form-control mx-2"
            v-model="todo.editText"
            @keyup.enter="saveTodo(todo)"
            @keyup.esc="todo.edit = false"
            @blur="todo.edit = false"
            ref="edit"
          />
          <span
            @dblclick="editTodo(todo)"
            v-else
            :class="{ done: todo.done }"
            style="flex-grow: 1"
            @mousedown.prevent
            >{{ todo.task }}</span
          >
          <button
            @click="clearTodo(todo.id)"
            class="btn btn-sm btn-danger mx-2"
          >
            Delete
          </button>
          <button
            v-if="!todo.edit"
            @click="editTodo(todo)"
            class="btn btn-sm btn-primary mx-2"
          >
            Edit
          </button>
          <div v-else class="d-flex">
            <button class="btn btn-sm btn-secondary" @click="todo.edit = false">
              Cancel
            </button>
            <button @click="saveTodo(todo)" class="btn btn-sm btn-primary mx-2">
              Save
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      task: "",
      todos: [],
      title: "Fey's batch"
    };
  },
  mounted() {
    const storedTodos = localStorage.getItem("todos");
    if (storedTodos) this.todos = JSON.parse(storedTodos);
  },
  methods: {
    addTodo() {
      const newTask = {
        task: this.task,
        id: Date.now(),
        done: false,
        edit: false,
        editText: ""
      };
      this.task && this.todos.push(newTask);
      this.task = "";
    },
    clearTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    saveTodo(todo) {
      todo.task = todo.editText;
      todo.done = false;
      todo.edit = false;
    },
    editTodo(todo) {
      todo.edit = true;
      todo.editText = todo.task;
      this.$nextTick(() => this.$refs.edit.focus());
    }
  },
  watch: {
    todos: {
      handler(todos) {
        const todosCopy = todos.map(todo => ({
          ...todo,
          edit: false
        }));

        localStorage.setItem("todos", JSON.stringify(todosCopy));
      },
      deep: true
    }
  }
};
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
.done {
  text-decoration: line-through;
}
</style>
