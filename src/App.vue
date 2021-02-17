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
        <li class="d-flex align-items-center justify-content-between w-25 mt-4">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ done: todo.done }">{{ todo.task }}</span>
          <button @click="clearTodo(todo.id)" class="btn btn-sm btn-danger">
            Delete
          </button>
          <button
            type="button"
            class="btn btn-primary"
            data-bs-toggle="modal"
            data-bs-target="#editModal"
          >
            Edit
          </button>
        </li>
      </ul>
    </div>
  </div>
  <!-- <div>
    <b-button v-b-modal.modal-1>Launch demo modal</b-button>

    <b-modal id="modal-1" title="BootstrapVue">
      <p class="my-4">Hello from modal!</p>
    </b-modal>
  </div> -->
  <!-- <div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          ...
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Close
          </button>
          <button type="button" class="btn btn-primary">Save changes</button>
        </div>
      </div>
    </div>
  </div> -->
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
        done: false
      };
      this.task && this.todos.push(newTask);
      this.task = "";
    },
    clearTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    updateLocalStorage(newTask) {
      const todos = JSON.parse(localStorage.getItem("todos"));
      todos.push(newTask);
      localStorage.setItem("todos", JSON.stringify(todos));
    }
  },
  created() {
    this.$watch(
      "todos",
      todos => {
        localStorage.setItem("todos", JSON.stringify(todos));
      },
      { deep: true }
    );
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
