<template>
  <li>
    <span class="item" 
    :class="todoItemClass"
    @click="toggleItem"> {{ todoItem.title }} </span>
    <button @click="removeTodo">삭제</button>
  </li>
</template>

<script lang="ts">
  import { Todo } from '@/App.vue';
  import Vue, { PropType } from 'vue'

  export default Vue.extend({
    props: {
      todoItem: Object as PropType<Todo>,
      index: Number
    },
    methods: {
      removeTodo() {
        this.$emit("remove", this.index);
      },
      toggleItem() {
        this.$emit("toggle", this.index, this.todoItem);
      }
    },
    computed: {
      todoItemClass(): string | null {
        return this.todoItem.completed ? 'complete' : null;
      }
    }
  })
</script>

<style scoped>
.item {
  cursor: pointer;
}
.complete {
  text-decoration: line-through;
}
</style>