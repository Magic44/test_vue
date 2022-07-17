<template>
  <div class="todo-footer" v-if="total">
    <label>
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ done }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="remove">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos"],
  methods: {
    remove() {
      this.$emit('removeAllDone');
    },
  },
  computed: {
    total() {
      return this.todos.length;
    },
    done() {
      // 演示用于统计数组中符合条件的元素个数
      return this.todos.reduce((sum, todo) => (todo.done ? ++sum : sum), 0);
    },
    isAll: {
      get() {
        return this.total === this.done && this.total > 0;
      },
      set(val) {
        this.$emit('changeAllTodo',val);
      },
    },
  },
};
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>