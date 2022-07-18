<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @add="add"></MyHeader>
        <MyList
          :todos="todos"
        ></MyList>
        <MyFooter
          :todos="todos"
          @deleteDone="deleteDone"
          @checkAll="checkAll"
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
    add(todo) {
      this.todos.unshift(todo);
    },
    // 改变todo的完成状态
    check(id) {
      this.todos.forEach(function (todo) {
        if (todo.id === id) {
          todo.done = !todo.done;
        }
      });
    },
    // 改变所有todo的完成状态
    checkAll(flag) {
      this.todos.forEach(function (todo) {
        todo.done = flag;
      });
    },
    // 根据id删除todo
    delete(id) {
      let index = this.todos.findIndex((todo) => todo.id === id);
      this.todos.splice(index, 1);
    },
    // 删除所有已完成的todo
    deleteDone() {
      let newArr = this.todos.filter(todo => !todo.done);
      this.todos = newArr
    },
    // 改变todo的编辑状态
    edit(id){
      this.todos.forEach(function (todo) {
        if (todo.id === id) {
          todo.isEdit = !todo.isEdit;
        }
      });
    },
    // 修改todo的名字
    update(data){
      this.todos.forEach(function (todo) {
        if (todo.id === data.id) {
          todo.name = data.name;
        }
      });
    }
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
  // 绑定事件
  mounted(){
    this.$bus.$on('check',this.check)
    this.$bus.$on('delete',this.delete)
    this.$bus.$on('edit',this.edit)
    this.$bus.$on('update',this.update)
  },
  // 解绑事件
  beforeDestroy() {
    this.$bus.$off('check')
    this.$bus.$off('delete')
    this.$bus.$off('edit')
    this.$bus.$off('update')
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

.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid #2f9ebd;
  margin-right: 5px;
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