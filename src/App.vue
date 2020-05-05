<template>
  <div class="todoapp" :class="state.nowShowing">
    <Header @create="onCreate" />
    <Main
      v-bind="state"
      @toggleAll="onToggleAll"
      @toggle="onToggle"
      @destroy="onDestroy"
      @update="onUpdate"
    />
    <Footer v-bind="state" @toggleShowing="onToggleShowing" @clearCompleted="onClearCompleted" />
  </div>
</template>

<script>
import { reactive } from "vue";
import Header from "./Header.vue";
import Main from "./Main.vue";
import Footer from "./Footer.vue";
import { ALL_TODOS } from "./constants";

export default {
  components: {
    Header,
    Main,
    Footer
  },
  setup() {
    const state = reactive({
      nowShowing: ALL_TODOS,
      todos: [
        { id: Date.now(), text: "hello", completed: false },
        { id: Date.now() + 1, text: "world", completed: true }
      ]
    });

    const onCreate = todo => {
      state.todos.push(todo);
    };

    const onToggleAll = completed => {
      state.todos.forEach(todo => {
        todo.completed = completed;
      });
    };

    const onToggle = id => {
      state.todos.forEach(todo => {
        if (todo.id === id) {
          todo.completed = !todo.completed;
        }
      });
    };

    const onDestroy = id => {
      const index = state.todos.findIndex(todo => todo.id === id);
      state.todos.splice(index, 1);
    };

    const onUpdate = ({ id, text }) => {
      const index = state.todos.findIndex(todo => todo.id === id);
      state.todos[index].text = text;
    };

    const onToggleShowing = nowShowing => {
      state.nowShowing = nowShowing;
    };

    const onClearCompleted = () => {
      state.todos = state.todos.filter(todo => !todo.completed);
    };

    return {
      state,
      onCreate,
      onToggleAll,
      onToggle,
      onDestroy,
      onUpdate,
      onToggleShowing,
      onClearCompleted
    };
  }
};
</script>