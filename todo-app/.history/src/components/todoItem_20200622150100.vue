<template>
    <div class="todo-item">

        <div class="todo-item-left">
          <input type="checkbox" v-model="completed" 
          @change="doneEdit">
          <div v-if="!editing" @dblclick="editTodo" 
          class="todo-item-label" :class="{ completed : 
          completed }" > {{ title }} </div>      
        <input v-else class="todo-item-edit" type=" text"
         v-model="title" @blur="doneEdit" 
         @keyup.enter="doneEdit" @keyup.esc="cancelEdit"
          v-focus >
        </div>
        <div>
            <button @click="pluralize"> Plural </button>
            <span class="remove-item" @click="removeTodo(index)">
                &times;
             </span>
          </div>
    </div>
</template>

<script>
export default {
    name:'todo-item',
    props: {
        todo: {
            type:Object,
            required:true,

        },
        index: {
            type:Number,
            required:true,
        },
        checkAll:{
            type:Boolean,
            required :true,
        
        }
        
    },
    data(){
        return {
            'id' : this.todo.id,
            'title': this.todo.title,
            'completed': this.todo.completed,
            'editing': this.todo.editing,
            'beforeEditCache':'',

        }
    },
    watch: {
        checkAll(){
          /*  if (this.checkAll) {
                this.completed = true
            }else{
                this.completed = this.todo.completed
            }
        }*/

        this.completed = this.checkAll ? true : this.todo.completed
        }
    },
    directives: {
        focus: {
            inserted: function (el) {
            el.focus()
            }
        }
    },
    methods: {
        removeTodo(index){
            eventBus.$emit('removeTodo', index)
        },
        editTodo(todo){/*Duzenleme methodu */
            this.beforeEditCache = this.title
            this.editing = true
        },
        doneEdit(){//Duzenlemeyi tamamlama methodu

            if(this.title.trim().length == '' ){
                //Bu kisim title tamamen silinip tekrar enter a basilirsa eski hale getiriyor
                this.title = this.beforeEditCache
            }
            this.editing = false
            eventBus.$emit('finishedEdit', {
                'index': this.index,
                'todo': {
                    'id': this.id,
                    'title': this.title,
                    'completed' : this.completed,
                    'editing' : this.editing,
                }
            })
        },
        cancelEdit() {//Esc tusuna basarak duzenlemeden cikma methodu
            this.title = this.beforeEditCache
            this.editing = false
        },
        pluralize() {
            eventBus.$emit('pluralize')
        },
    }
}
</script>