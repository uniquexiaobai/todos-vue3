<template>
  <li :class="{completed: todo.completed, editing: editing}">
    <div class="view">
      <input class="toggle" type="checkbox" v-model="todo.completed" @click="onToggle" />
      <label v-on:dblclick="onEdit">{{ todo.text }}</label>
      <button class="destroy" @click="onDestroy(todo.id)" />
    </div>

    <input
      ref="input"
      class="edit"
      v-model.trim="editingText"
      @blur="onBlur"
      @keyup.enter="onSubmit"
    />
  </li>
</template>

<script>
import { ref, computed, watchEffect } from "vue";
import { ACTIVE_TODOS, COMPLETED_TODOS } from "./constants";

export default {
  props: {
    todo: {
      type: Object,
      default: () => ({})
    }
  },
  setup(props, { emit }) {
    const editing = ref(false);
    const editingText = ref(props.todo.text);
    const input = ref(null);

    watchEffect(() => {
      if (editing) {
        input.value && input.value.focus();
      }
    });

    const onEdit = () => {
      editing.value = true;
    };

    const onSubmit = () => {
      onBlur();
    };

    const onToggle = () => {
      emit("toggle", props.todo.id);
    };

    const onDestroy = id => {
      emit("destroy", id);
    };

    const onBlur = () => {
      if (editingText.value) {
        emit("update", { id: props.todo.id, text: editingText });
        editing.value = false;
      } else {
        onDestroy(props.todo.id);
      }
    };

    return {
      input,
      todo: props.todo,
      editing,
      editingText,
      onEdit,
      onSubmit,
      onToggle,
      onDestroy,
      onBlur
    };
  }
};
</script>