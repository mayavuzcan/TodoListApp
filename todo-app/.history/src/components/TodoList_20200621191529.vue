<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done"
       v-model="newTodo" @keyup.enter="addTodo">
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item" >
        <div class="todo-item-left">
          <div v-if="!todo.editing" @dblclick="editTodo(todo)" 
          class="todo-item-label">{{ todo.title }}</div>      
        <input v-else class="todo-item-edit" type=" text"
         v-model="todo.title" @blur="doneEdit(todo)" 
         @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)"
          v-focus >
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
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
      beforeEditCache: '',
      /* `beforeEditCache` double click isleminden sonra eger 
      duzenleme yapilmadan cikilmak isteniyorsa eski hale 
      gelmesi icin olusturulan data*/

      todos: [
        {
          'id': 1,
          'title': 'Finish Vue Screencast',
          'completed': false,
          'editing': false,
        },
         {
          'id': 2,
          'title': 'Take over world',
          'completed': false,
          'editing': false,
        }
      ]
    }
  },
   directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
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
    },
    editTodo(todo){/*Duzenleme methodu */
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    doneEdit(todo){//Onaylama methodu
       if(todo.title.trim().length == 0 ){
        return
      }
      todo.editing = false
    },
    removeTodo(index){//Silme methodu
      this.todos.splice(index, 1)
    },
     cancelEdit(todo) {//Esc tusuna basarak duzenlemeden cikma methodu
      todo.title = this.beforeEditCache
      todo.editing = false
    },
  }
}
</script>

<style lang="scss">

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus{
    outline: 0;
  }
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
  .todo-item-left { // later
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }
  .todo-item-edit {
    font-size: 24px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc; //override defaults
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    &:focus {
      outline: none;
    }
  }

</style>
