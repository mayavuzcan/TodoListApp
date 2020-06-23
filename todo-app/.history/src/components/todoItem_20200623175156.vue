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
    created(){
        eventBus.$on('pluralize', this.handlePluralize)
    },
    beforeDestroy(){
        eventBus.$off('pluralize', this.handlePluralize)

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
          const index = this.$store.state.todos.findIndex(item => item.id == id)
          this.$store.state.todos.splice(index, 1)
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
            const index = this.$store.state.todos.findIndex(item => item.id == this.id)
            this.$store.state.todos.splice(index,1,{
                    'id': this.id,
                    'title': this.title,
                    'completed' : this.completed,
                    'editing' : this.editing,
            })
            // eventBus.$emit('finishedEdit', {
            //     'index': this.index,
            //     'todo': {
            //         'id': this.id,
            //         'title': this.title,
            //         'completed' : this.completed,
            //         'editing' : this.editing,
            //     }
            // })
        },
        cancelEdit() {//Esc tusuna basarak duzenlemeden cikma methodu
            this.title = this.beforeEditCache
            this.editing = false
        },
        pluralize() {
            /*bu kisim pluralize islemi icin daha sonra kaldirilacak*/
            eventBus.$emit('pluralize')
        },
        handlePluralize() {

            /*bu kisim pluralize islemi icin daha sonra kaldirilacak*/
            this.title = this.title + 's'
            const index = this.$store.state.todos.findIndex(item => item.id == this.id)
            this.$store.state.todos.splice(index,1,{
                    'id': this.id,
                    'title': this.title,
                    'completed' : this.completed,
                    'editing' : this.editing,
            })
    }
    }
}
</script>