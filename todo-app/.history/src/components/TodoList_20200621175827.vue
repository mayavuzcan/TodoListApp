<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done"
       v-model="newTodo" @keyup.enter="addTodo">/* @keyup  div deki hareketi methoda gondermeye yarar */ 
      
      <div v-for="todo in todos" :key="todo.id" class="todo-item" >
        {{ todo.title }}
      </div>
  </div>
</template>

<script>
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

}

</style>
