<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done">
      Todo List goes here
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="scss">

.todo-input {
  width: 600px;
  padding: 10px 520px;
  font-size: 20px;
  margin-bottom: 16px;

  &:focus{
    outline: 0;
  }

}

</style>
