<template>
 <div>  <label><input type="checkbox" :checked="!anyRemaining"
        @change="checkAllTodos">Check All</label> </div>
</template>

<script>
export default {
    name: 'todo-check-all',
    props:{
        anyremaining: {
            type: Boolean,
            required: true,
        }
    }
}
</script>