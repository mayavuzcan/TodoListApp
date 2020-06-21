<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done"
       v-model="newTodo" @keyup.enter="addTodo">
      <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item" >
        <div class="todo-item-left">
          <input type="checkbox" v-model="todo.completed" >
          <div v-if="!todo.editing" @dblclick="editTodo(todo)" 
          class="todo-item-label" :class="{ completed : 
          todo.completed }" > {{ todo.title }} </div>      
        <input v-else class="todo-item-edit" type=" text"
         v-model="todo.title" @blur="doneEdit(todo)" 
         @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)"
          v-focus >
        </div>
        <div class="remove-item" @click="removeTodo(index)">
          &times;
        </div>
      </div>
        
      <div class="extra-container">
        <div>  <label><input type="checkbox" :checked="!anyRemaining"
        @change="checkAllTodos">Check All</label> </div>
        <div>{{remaining}} items left</div>
      </div>

      <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>

      <div>
        Clear Completed
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

      filter:'all',

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
  computed:{
    remaining(){
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining(){
      return this.remaining != 0
    },
    todosFiltered(){

       if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos

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
    doneEdit(todo){//Duzenlemeyi tamamlama methodu

       if(todo.title.trim().length == '' ){
         //Bu kisim title tamamen silinip tekrar enter a basilirsa eski hale getiriyor
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    removeTodo(index){//Silme methodu
      this.todos.splice(index, 1)
    },
    checkAllTodos(){
      this.todos.forEach((todo) => todo.completed = 
      event.target.checked)
    },
     cancelEdit(todo) {//Esc tusuna basarak duzenlemeden cikma methodu
      todo.title = this.beforeEditCache
      todo.editing = false
    },
  }
}
</script>

<style lang="scss">
 @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");


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
  .todo-item-left { 
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

   .completed {
     //title ustlerini cizmek icin
    text-decoration: line-through;
    color: grey;
  }

  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }

  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    &:hover {
      background: lightgreen;
    }
    &:focus {
      outline: none;
    }
  }

  .active {
    background: lightgreen;
  }

</style>
