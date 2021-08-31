<template>
    <li class = "myListItem" :class="{done: todo.checked}">
      <input type="checkbox" class="checkboxItem" :checked="todo.checked" @change="checkboxValue">
        {{todo.title}}
      <button id="deleteButton" class="btn-close" aria-label="Close" @click="removeTodo"></button>
    </li>
</template>

<script>
export default {
  name: 'myListItem',
  props: {
      todo: {
          type: Object,
          required: true,
          default() {
              return {}
          }
      }
    },
    methods:{
      checkboxValue(){
        this.$emit('id-check', {
          id: this.todo.id,
          value: !this.todo.checked
        });
      },
      removeTodo() {
        this.$emit('remove-click-button-item', {
          id: this.todo.id
        })
      },
    }
}
</script>

<style>
.btn-close {
  margin: 10px;
  display: none;
  position: absolute;
  right: 0;
  bottom: 0;
}

.myListItem{
  padding: 10px;
  display: block;
  position: relative;
}

.checkboxItem{
  margin: 10px;
}

.done {
  text-decoration: line-through;
}
</style>