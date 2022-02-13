<template>
  <div class="TodoListItem">
    <div class="erjiItem">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="choose(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        ref="inputTitle"
        v-show="todo.isEdit"
        type="text"
        :value="todo.title"
        @blur="removeBlur(todo, $event)"
      />
      <i class="el-icon-delete" @click="handleDelete(todo.id)"></i>
      <i v-show="!todo.isEdit" class="el-icon-edit" @click="handleEdit(todo)"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoListItem",
  props: ["todo"],
  methods: {
    //勾选or取消勾选
    //通知app，将id值相同的completed取反
    choose(id) {
      // this.updataTodo(id);
      this.$bus.$emit("updataTodo", id);
    },
    //删除
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        // this.deleteTodo(id);
        this.$bus.$emit("deleteTodo", id);
        // console.log(id);
      }
    },
    //编辑
    handleEdit(todo) {
      if (todo.hasOwnProperty("isEdit")) {
        todo.isEdit = true;
      } else {
        this.$set(todo, "isEdit", true);
      }
       //自动获得焦点
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
    },
    //失去焦点后(真正的修改函数)
    removeBlur(todo, e) {
      if (!e.target.value.trim()) {
        alert("输入不能为空！");
        todo.isEdit = false;
      } else {
        this.$bus.$emit("updateTitle", todo, e.target.value);
        todo.isEdit = false;
      }
        
    },
  },
};
</script>

<style scoped>
.erjiItem {
  position: relative;
  width: 100%;
  border-bottom: 1px solid gray;
  height: 30px;
  line-height: 30px;
  margin: 10px 0px;
}

.erjiItem:hover {
  background-color: #ddd;
}

.erjiItem input:first-child{
  margin-right: 10px;
}

.el-icon-delete {
  position: absolute;
  line-height: 25px;
  right: 20px;
  height: 25px;
  top: 2px;
  cursor: pointer;
}

.el-icon-edit {
  position: absolute;
  line-height: 25px;
  right: 50px;
  height: 25px;
  top: 2px;
  cursor: pointer;
}
.erjiItem:hover button {
  display: block;
}


</style>

