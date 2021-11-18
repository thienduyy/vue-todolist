<template>
  <div id="todo-item">
    <div class="item-left">
      <input
        class="item-checkbox"
        v-model="completed"
        type="checkbox"
        @change="doneEdit"
      />
      <div
        class="item-label"
        :class="{ completed: completed }"
        v-if="!editing"
        @dblclick="editTodo"
      >
        {{ title }}
      </div>
      <input
        v-else
        type="text"
        class="item-edit"
        v-model="title"
        @keyup.enter="doneEdit"
        @blur="doneEdit"
        @keyup.esc="cancelEdit"
        v-focus
      />
    </div>
    <div class="remove-item" @click="removeTodo(index)">&times;</div>
  </div>
</template>

<script>
export default {
  name: "todo-item",
  props: {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  data: function () {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforeEdit: "",
    };
  },
  methods: {
    removeTodo(index) {
      this.$emit("removedTodo", index);
    },
    editTodo: function () {
      this.beforeEdit = this.title;
      this.editing = true;
    },
    cancelEdit: function () {
      this.title = this.beforeEdit;
      this.editing = false;
    },
    doneEdit() {
      if (this.title.trim().length === 0) {
        this.title = this.beforeEdit;
      }
      this.editing = false;

      this.$emit("finishedEdit", {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          completed: this.completed,
          editing: this.editing,
        },
      });
    },
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
.item-left {
  display: flex;
  align-items: center;
}

.item-checkbox {
  margin-right: 20px;
}
#todo-item {
  border-top: 1px solid #6666663a;
  padding: 8px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
