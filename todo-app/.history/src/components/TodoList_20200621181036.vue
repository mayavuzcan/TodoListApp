<template>

  <div>
    
      <input type="text" class="todo-input" placeholder="What needs to be done"
       v-model="newTodo" @keyup.enter="addTodo">

      <div v-for="todo in todos" :key="todo.id" class="todo-item" >
        <div>      
        {{ todo.title }}
        </div>
        
          &times;
        
      </div>
  </div>
</template>

<script>
/* @keyup  div deki hareketi methoda gondermeye yarar */ 
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      todos: [
        {
          'id': 1,
          'title': 'Finish Vue Screencast',
          'completed': false,
        },
         {
          'id': 2,
          'title': 'Take over world',
          'completed': false,
        }
      ]
    }
  },
  methods: { /* Bu method ile @keyup.enter ile yakalanan islem burada gerceklestirilir */
    addTodo(){

      if(this.newTodo.trim().length == 0 ){
        return
      }

      /*Bu kisimda yakalanan methodun icin todos objesine ekleme yapiliyor */
      this.todos.push({
        id:this.idForTodo,
        title:this.newTodo,
        completed:false,
      })

      this.newTodo = ''
      this.idForTodo++
    }
  }
}
</script>

<style lang="scss">

.todo-input {
  width: 500px;
  padding: 10px 20px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus{
    outline: 0;
  }
  .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-content: center;
    justify-content: space-between;
  }
  .remove-item {
    cursor: pointer;
    margin-left: 14px;
    &:hover{
      color: black;
    }
  }

}

</style>
