<template>
  <transition name="todo" appear>
    <li>
      <label>
        <input type="checkbox" @change="handlerChange" :checked="todo.done" />
        <span v-show="!todo.isEdit">{{ todo.name }}</span>
        <input
          v-show="todo.isEdit"
          type="text"
          :value="todo.name"
          @blur="handlerBlur"
          ref="inputTitle"
        />
      </label>
      <button
        class="btn btn-danger"
        @click="handlerDelete"
        v-show="!todo.isEdit"
      >
        删除
      </button>
      <button class="btn btn-edit" @click="handlerEdit" v-show="!todo.isEdit">
        编辑
      </button>
    </li>
  </transition>
</template>

<script>
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    handlerChange() {
      this.$bus.$emit("check", this.todo.id);
    },
    handlerEdit() {
      this.$bus.$emit("edit", this.todo.id);
      this.$nextTick(() => {
        this.$refs.inputTitle.focus();
      });
    },
    handlerDelete() {
      if (confirm("亲，确定要删除该任务吗？")) {
        this.$bus.$emit("delete", this.todo.id);
      }
    },
    handlerBlur(e) {
      this.$bus.$emit("edit", this.todo.id);
      if (e.target.value === "") return alert("任务名不可为空！");
      this.$bus.$emit("update", {
        id: this.todo.id,
        name: e.target.value,
      });
    },
  },
};
</script>

<style scoped>
/* 进入时要激活的样式 */
.todo-enter-active {
  animation: atguigu 1s;
}
/* 退出时要激活的样式 */
.todo-leave-active {
  animation: atguigu 1s reverse;
}
/* 提前准备好动画 */
@keyframes atguigu {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0px);
  }
}
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>