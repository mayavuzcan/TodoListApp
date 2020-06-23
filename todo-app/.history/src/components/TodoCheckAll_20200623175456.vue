<template>
    <div>  <label><input type="checkbox" :checked="!anyRemaining"
    @change="allChecked">Check All</label> </div>
</template>

<script>
export default {
    name: 'todo-check-all',
    computed:{
        anyRemaining() {
            return this.$store.getter.anyRemaining
        }
    },
    methods: {
        allChecked(){
            eventBus.$emit('checkAllChanged',this.anyRemaining)
        }
    }
}
</script>