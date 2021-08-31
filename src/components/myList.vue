<template>
<div class="containerList">
  <ul id="myList">
    <my-list-item 
    v-for="todo in todos" 
    :key="todo.id" 
    :todo="todo"
    @id-check="$emit('id-check', $event)"
    @remove-click-button-item="$emit('remove-click-button-item', $event)"
    ></my-list-item>
  </ul>
</div>
</template>

<script>
//import { computed } from '@vue/reactivity'
import myListItem from './myListItem.vue'
export default {
  name: 'myList',
  props: {
    todos: {
      type: Array,
      required: true,
      default() {
        return []
      }
    }
  },
  components: {
    myListItem
  }
}
</script>

<style>
#myList {
  list-style-type: none;
}

.myListItem:hover .btn-close {
  display: block;
}

.containerList {
  width: 100%;
  height: 100%;
}
</style>