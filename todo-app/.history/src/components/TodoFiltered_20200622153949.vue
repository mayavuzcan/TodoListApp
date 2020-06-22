<template>
    <div>
        <button :class="{ active: filter == 'all' }"
         @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }"
         @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }"
         @click="filter = 'completed'">Completed</button>
    </div>
</template>

<script>
export default {
    name: 'todo-filtered',
    data() {
        return{
            'filter': 'all',
        }
    },
    methods: {
        allChecked(){
            eventBus.$emit('checkAllChanged',this.anyRemaining)
        }
    }
}
</script>