<template>
  <div class="TodoListHeader">
    <h1 class="header">Todo清单</h1>
    <span class="headerSpan">欢迎你：{{ user }}</span>
    <span class="exitSpan" @click="exit">点我退出</span>
    <input
      class="enterList"
      type="text"
      placeholder="请输入你的任务名称，按回车键确认"
      v-model="title"
      @keyup.enter="addtodo()"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "TodoListHeader",
  data() {
    return {
      title: "",
      user: window.localStorage.getItem("user"),
    };
  },
  methods: {
    addtodo() {
      //判断数据是否为空
      if (!this.title.trim()) return alert("输入不能为空");
      //把数据包装成一个对象
      const todoObj = { id: nanoid(), title: this.title, completed: false };
      // console.log(todoObj);
      this.$emit("receive", todoObj);
      this.title = "";
    },
    exit() {
      this.$bus.$emit("exitToLogin");
    },
  },
  // props:['receive']
};
</script>

<style scoped>
.TodoListHeader {
  margin: 15px;
  /* text-align: center; */
  z-index: 10;
}

.enterList {
  width: 99%;
  height: 30px;
  margin-top: 5px;
  border: 1px solid gray;
}

.header {
  display: inline;
}

.headerSpan {
  margin-left: 280px;
}

.exitSpan {
  margin-left: 30px;
  cursor: pointer;
}
</style>