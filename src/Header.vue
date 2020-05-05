<template>
  <header class="header">
    <h1>todos</h1>

    <input
      class="new-todo"
      placeholder="What needs to be done?"
      ref="input"
      v-model.trim="text"
      @keyup.enter="onSubmit"
    />
  </header>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  directives: {
    focus: {
      inserted(el) {
        el.focus();
      }
    }
  },
  setup(props, { emit }) {
    const text = ref("");
    const input = ref(null);

    onMounted(() => {
      input.value.focus();
    });

    const onSubmit = () => {
      if (text.value) {
        const todo = {
          id: Date.now(),
          text: text.value
        };

        emit("create", todo);
        text.value = "";
      }
    };

    return { input, text, onSubmit };
  }
};
</script>