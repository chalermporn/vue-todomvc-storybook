<template>
  <li :class="{ completed: item.completed, editing: item === editedTodo }">
    <div class="view">
      <input class="toggle" type="checkbox"
        :checked="item.completed"
        @click="toggleItem">
      <label @dblclick="editTodo(item)">{{ item.title }}</label>
      <button class="destroy" @click="$emit('deleteItem')"></button>
    </div>
    <input class="edit" v-model="editTitle"
      v-todo-focus="item === editedTodo"
      @blur="doneEdit(item)"
      @keyup.enter="doneEdit(item)"
      @keyup.esc="cancelEdit(item)">
  </li>
</template>

<script>
export default {
  data () {
    return {
      editTitle: '',
      beforeEditCache: '',
      editedTodo: null
    }
  },
  props: {
    item: {
      type: Object
    }
  },
  methods: {
    toggleItem (e) {
      e.preventDefault()
      this.$emit('toggleItem')
    },
    editTodo (item) {
      this.beforeEditCache = item.title
      this.editTitle = item.title
      this.editedTodo = item
    },
    doneEdit (todo) {
      if (!this.editedTodo) {
        return
      }
      this.editedTodo = null
      this.editTitle = this.editTitle.trim()
      if (this.editTitle) {
        this.$emit('editItem', this.editTitle)
      } else {
        this.$emit('deleteItem')
      }
    },
    cancelEdit (item) {
      this.editedTodo = null
      item.title = this.beforeEditCache
    }
  },
  directives: {
    'todo-focus' (el, binding) {
      if (binding.value) {
        el.focus()
      }
    }
  }
}
</script>
