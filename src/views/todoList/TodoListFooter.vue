<template>
  <div class="TodoListFooter" v-show="total">
    <input type="checkbox" :checked="isChecked" @change="isAll" />
    <span>&nbsp;&nbsp;&nbsp;&nbsp;已完成{{ doneTotal }}/全部{{ total }}</span>
    <input type="button" @click="alltodo()" value="清除已完成任务" />
  </div>
</template>

<script>
export default {
  name: "TodoListFooter",
  props: ["todos", "deleteAllTrueTodo", "isTodoAll"],
  data() {
    return {};
  },
  methods: {
    alltodo() {
      if (confirm("确定删除吗？")) {
        // console.log('1');
        this.$emit('deleteAllTrueTodo')
      }
    },

    // 全选按钮（点击以后全部都选or全都不选）
    isAll(e) {
      // this.isTodoAll(e.target.checked);
      this.$emit('isTodoAll',e.target.checked);
    },
  },
  computed: {
    total() {
      return this.todos.length;
    },

    doneTotal() {
      return this.todos.reduce((pre, current) => {
        // console.log('##',pre,current)
        return pre + (current.completed ? 1 : 0);
      }, 0);
    },
    isChecked() {
      return this.doneTotal == this.total && this.total > 0;
    },
  },
};
</script>


<style scoped>
.TodoListFooter {
  position: absolute;
  padding-top: 15px;
  margin: 25px 15px;
  z-index: 10;
  bottom: 0px;
}

.TodoListFooter input[type="button"] {
  background-color: rgb(179, 11, 11);
  color: white;
  font-weight: bold;
  margin-left: 400px;
}
</style>