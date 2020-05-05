<template>
  <footer v-if="todos.length" class="footer" :class="nowShowing">
    <span class="todo-count">
      <strong>{{activeTodoCount}}</strong>
      item{{activeTodoCount === 1 ? '' : 's'}} left
    </span>

    <ul class="filters">
      <li>
        <a
          @click="onToggleShowing(ALL_TODOS)"
          :style="{cursor: 'pointer'}"
          :class="{selected: nowShowing === ALL_TODOS}"
        >All</a>
      </li>
      {{' '}}
      <li>
        <a
          @click="onToggleShowing(ACTIVE_TODOS)"
          :style="{cursor: 'pointer'}"
          :class="{selected: nowShowing === ACTIVE_TODOS}"
        >Active</a>
      </li>
      {{' '}}
      <li>
        <a
          @click="onToggleShowing(COMPLETED_TODOS)"
          :style="{cursor: 'pointer'}"
          :class="{selected: nowShowing === COMPLETED_TODOS}"
        >Completed</a>
      </li>
      {{' '}}
    </ul>
    <button class="clear-completed" @click="onClearCompleted">Clear completed</button>
  </footer>
</template>

<script>
import { ref, watchEffect, computed } from "vue";
import { ALL_TODOS, ACTIVE_TODOS, COMPLETED_TODOS } from "./constants";

export default {
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
    const activeTodoCount = computed(() => {
      return props.todos.reduce(
        (total, todo) => (todo.completed ? total : total + 1),
        0
      );
    });
    const nowShowing = computed(() => {
      return props.nowShowing;
    });
    const todos = computed(() => {
      return props.todos;
    });

    const onToggleShowing = nowShowing => {
      emit("toggleShowing", nowShowing);
    };
    const onClearCompleted = () => {
      emit("clearCompleted");
    };

    return {
      ALL_TODOS,
      ACTIVE_TODOS,
      COMPLETED_TODOS,
      nowShowing,
      todos,
      activeTodoCount,
      onToggleShowing,
      onClearCompleted
    };
  }
};
</script>