<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList
          :todos="todos"
        ></MyList>
        <MyFooter
          :todos="todos"
          @removeAllDone="removeAllDone"
          @changeAllTodo="changeAllTodo"
        ></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  components: {
    MyHeader,
    MyFooter,
    MyList,
  },
  methods: {
    // 数据在哪，那么操作数据的方法就在哪
    // 添加一个todo
    addTodo(todo) {
      this.todos.unshift(todo);
    },
    // 根据todo的id改变todo的完成状态
    changeTodo(id) {
      this.todos.forEach(function (todo) {
        if (todo.id === id) {
          todo.done = !todo.done;
        }
      });
    },
    // 改变所有todo的完成状态
    changeAllTodo(flag) {
      this.todos.forEach(function (todo) {
        todo.done = flag;
      });
    },
    // 根据id删除todo
    removeTodo(id) {
      let index = this.todos.findIndex((todo) => todo.id === id);
      this.todos.splice(index, 1);
    },
    // 删除所有已完成的todo
    removeAllDone() {
      let newArr = this.todos.filter(todo => !todo.done);
      console.log(newArr);
      this.todos = newArr
    },
  },
  watch: {
    // 浏览器本地存储
    todos: {
      deep: true,
      immediate: true,
      handler(val) {
        localStorage.setItem("todos", JSON.stringify(val));
      },
    },
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  mounted(){
    // 使用全局事件总线
    this.$bus.$on('removeTodo',this.removeTodo)
    this.$bus.$on('changeTodo',this.changeTodo)
  },
  beforeDestroy() {
    // 在beforeDestroy钩子中解绑事件
    this.$bus.$off('removeTodo')
    this.$bus.$off('changeTodo')
  },
};
</script>

<style>
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>