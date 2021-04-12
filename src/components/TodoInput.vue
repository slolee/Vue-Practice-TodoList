<template>
  <div>
    <label for="todo-input">오늘 할 일 : </label>
    <input id="todo-input" type="text" :value="item" @input="handleInput">
    <!-- <input id="todo-input" type="text" :value="$attrs.value" @input="handleInput"> -->
    <button type="button" @click="addTodo">ADD</button>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: {
    item: {
      type: String,
      required: true
    },
    temp: {
      type: Number,
    }
  },
  methods: { // 위에서 내려주기만 하면됨. 받을 준비가 되어있음을 말함.
    addTodo() {
      this.$emit("add");
    },
    handleInput(event: InputEvent) {
      console.log(event);
      // if(event.target == null) {
      //   return;
      // }

      const eventTarget = event.target as HTMLInputElement; // Unsafe, But 매우 편리함.
      this.$emit("input", eventTarget.value);
    },
  },
  mounted() {
    console.log(this.temp);
  }
});
</script>

