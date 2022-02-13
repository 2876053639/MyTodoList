<template>
    <div class="TodoListList">
          <TodoListItem v-for="todo in todos" :key='todo.id' :todo='todo'/>
      </div>
</template>

<script>
    import TodoListItem from './TodoListItem.vue'
    export default {
        name:'TodoListList',
        data(){
            return{
                
            }
        },
        components:{
            TodoListItem
        },
        props:['todos']
    }
</script>

<style scoped>
    .TodoListList{
    margin: 0px 15px;
    z-index: 10;
  }
</style>