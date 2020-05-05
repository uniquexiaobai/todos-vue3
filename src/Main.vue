<template>
  <div class="main">
    <input class="toggle-all" type="checkbox" v-model="noActiveTodo" @change="onToggleChange" />
    <ul class="todo-list">
      <Item
        v-for="todo in showTodos"
        :key="todo.id"
        :todo="todo"
        @toggle="payload => onAction('toggle', payload)"
        @destroy="payload => onAction('destroy', payload)"
        @update="payload => onAction('update', payload)"
      />
    </ul>
  </div>
</template>

<script>
import { ref, watchEffect, computed } from "vue";
import { ACTIVE_TODOS, COMPLETED_TODOS } from "./constants";
import Item from "./item.vue";

export default {
  components: {
    Item
  },
  props: {
    nowShowing: {
      required: true,
      type: String
    },
    todos: {
      required: true,
      type: Array
    }
  },
  setup(props, { emit }) {
    const noActiveTodo = computed(() => {
      return !props.todos.reduce(
        (total, todo) => (todo.completed ? total : total + 1),
        0
      );
    });
    const showTodos = computed(() => {
      return props.todos.filter(todo => {
        switch (props.nowShowing) {
          case ACTIVE_TODOS:
            return !todo.completed;
          case COMPLETED_TODOS:
            return todo.completed;
          default:
            return true;
        }
      });
    });

    const onToggleChange = () => {
      emit("toggleAll", !noActiveTodo.value);
    };

    const onAction = (type, payload) => {
      emit(type, payload);
    };

    return {
      showTodos,
      noActiveTodo,
      onToggleChange,
      onAction
    };
  }
};
</script>