<template>
  <div class="todo-task">
    <p>TODOS</p>
    <input
      type="text"
      class="input"
      placeholder="What needs to be done ?"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <div class="extra-container">
      <div>{{ remaining }} items left</div>
      <div class="filter">
        <button
          class="button"
          :class="{ active: filter === 'all' }"
          @click="filter = 'all'"
        >
          All
        </button>
        <button
          class="button"
          :class="{ active: filter === 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          class="button"
          :class="{ active: filter === 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
    </div>
    <div class="todo-list">
      <todo-item
        v-for="(todo, index) in todosFiltered"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @removedTodo="removeTodo"
        @finishedEdit="doneEdit"
      >
      </todo-item>
    </div>
    <div class="footer">
      <Button class="button-clear" @click="clearCompleted"
        >Clear Completed</Button
      >
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem.vue";
export default {
  name: "TodoList",
  data() {
    return {
      newTodo: "",
      idForTodo: 4,
      beforeEdit: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Learn Vuejs",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "Create a project",
          completed: false,
          editing: false,
        },
        {
          id: 3,
          title: "Finished",
          completed: false,
          editing: false,
        },
      ],
    };
  },
  computed: {
    remaining() {
      return this.todos.filter((item) => !item.completed).length;
    },
    todosFiltered() {
      if (this.filter === "all") {
        return this.todos;
      } else if (this.filter === "active") {
        return this.todos.filter((item) => !item.completed);
      } else {
        return this.todos.filter((item) => item.completed);
      }
    },
  },
  methods: {
    addTodo: function () {
      if (this.newTodo.trim().length === 0) return 0;
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      });
      this.newTodo = "";
      this.idForTodo++;
    },

    removeTodo: function (id) {
      this.todos.splice(id, 1);
    },

    editTodo: function (item) {
      this.beforeEdit = item.title;
      item.editing = true;
    },

    cancelEdit: function (item) {
      item.title = this.beforeEdit;
      item.editing = false;
    },

    doneEdit(data) {
      // if (item.title.trim().length === 0) {
      //   item.title = this.beforeEdit;
      // }
      // item.editing = false;
      this.todos.splice(data.index, 1, data.todo);
      console.log("-------", this.todos);
    },
    finishedEdit: function ({ index, todo }) {
      // console.log(data);
      this.todos.splice(index, 1, todo);
    },
    clearCompleted: function () {
      this.todos = this.todos.filter((item) => !item.completed);
    },
  },

  components: {
    TodoItem,
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus();
      },
    },
  },
};
</script>

<style>
.button {
  margin-left: 5px;
  background: transparent;
  border: 0px;
  color: rgb(77, 121, 77);
  padding: 8px;
  border-radius: 8px;
}
.button:hover {
  background: rgba(133, 192, 133, 0.432);
}
.button:focus {
  background: rgba(133, 192, 133, 0.432);
}
.button-clear {
  width: 400px;
  background: rgba(65, 158, 65, 0.651);
  color: #fff;
  border: 0px;
  padding: 4px 0;
  margin-top: 10px;
  border-radius: 8px;
  font-size: 18px;
}

.button-clear:hover {
  background: rgba(70, 146, 70, 0.521);
}
.todo-task {
  max-width: 400px;
  margin: 0 auto;
}
.todo-list {
  text-align: start;
  margin-top: 15px;
}
.extra-container {
  text-align: start;
  margin-top: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.input {
  width: 400px;
  height: 35px;
}
.input:focus {
  outline: none;
}

.item-edit {
  max-width: fit-content;
  height: 30px;
}
.item-edit:focus {
  outline: none;
}

.remove-item {
  cursor: pointer;
}
.remove-item:hover {
  color: red;
}

.completed {
  text-decoration: line-through;
  color: gray;
}
</style>
