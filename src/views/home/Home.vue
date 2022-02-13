<template>
  <div id="home">
    <TodoListHeader @receive="receive" />
    <hr />
    <TodoListList :todos="todos" />
    <div class="TodoListFooterMengBan"></div>
    <TodoListFooter
      :todos="todos"
      @deleteAllTrueTodo="deleteAllTrueTodo"
      @isTodoAll="isTodoAll"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import TodoListFooter from "../todoList/TodoListFooter.vue";
import TodoListHeader from "../todoList/TodoListHeader.vue";
import TodoListList from "../todoList/TodoListList.vue";
export default {
  name: "Home",
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    //修改todo中的title
    updateTitleTodo(todo, title) {
      todo.title = title;
    },
    //添加一个todo
    //接收header中传过来的值放到数组
    receive(value) {
      // console.log('我接收到了header的值',value);
      this.todos.unshift(value);
      // console.log(value)
    },
    //修改todo的completed
    //拿到completed值，把这个函数给item这个孙子使用
    updataTodo(id) {
      //遍历todos
      this.todos.forEach((todo) => {
        if (todo.id == id) todo.completed = !todo.completed;
      });
    },
    //删除一个todo
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => {
        return todo.id !== id;
      });
    },
    //删除全部勾选的todo
    deleteAllTrueTodo() {
      this.todos = this.todos.filter((todo) => {
        return !todo.completed;
      });
    },
    //全选or取消全选
    isTodoAll(completed) {
      this.todos.forEach((todo) => {
        todo.completed = completed;
      });
    },
    //注销
    exit() {
      this.$router.push({
        name: "Login",
      });
    },
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      },
    },
  },
  components: {
    TodoListFooter,
    TodoListHeader,
    TodoListList,
  },
  mounted() {
    this.$bus.$on("updataTodo", this.updataTodo);
    this.$bus.$on("deleteTodo", this.deleteTodo);
    this.$bus.$on("updateTitle", this.updateTitleTodo);
    this.$bus.$on("exitToLogin", this.exit);
  },
  beforeDestroy() {
    this.$bus.$off("updataTodo");
    this.$bus.$off("deleteTodo");
    this.$bus.$off("updateTitle");
  },
};
</script>
<style scoped>
#home {
  z-index: 10;
  position: absolute;
  left: 50%;
  top: 10%;
  width: 700px;
  height: auto;
  min-height: 550px;
  background-color: rgb(231, 252, 255);
  border-radius: 10px;
  transform: translateX(-50%);
}
.TodoListFooterMengBan{
  height: 55px;
}
</style>