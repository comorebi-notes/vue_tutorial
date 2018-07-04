<template>
  <div>
    <h2>{{ msg }}</h2>
    <form>
      <button @click="addTask()">ADD TASK</button>
      <button @click="deleteFinishedTasks()">DELETE FINISHED TASKS</button>
      <p>input: <input type="text" v-model="newTask"></p>
      <p>tasks: {{ newTask }}</p>
    </form>
    <div class="task-list">
      <label
        class="task-list__item"
        v-for="task in tasks"
        v-bind:class="{ checked: task.done }"
      >
        <input type="checkbox" v-model="task.done">
        <input type="checkbox" v-model="task.editing">
        <input v-if="task.editing" v-model="task.text" @keyup.enter="todo.editing = false">
        <span v-else>{{ task.text }}</span>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data () {
    return {
      msg: 'Todo Application',
      tasks: [
        { text: 'vue-router',  done: false, editing: false },
        { text: 'vuex',        done: false, editing: false },
        { text: 'vue-loader',  done: false, editing: false },
        { text: 'awesome-vue', done: true,  editing: false }
      ],
      newTask: ''
    }
  },
  methods: {
    addTask (event) {
      let text = this.newTask && this.newTask.trim()
      if (!text) return

      this.tasks.push({
        text,
        done: false,
        editing: false
      })
      this.newTask = ''
    },
    deleteFinishedTasks () {
      const newTasks = []
      this.tasks.forEach(task => {
        if (!task.done) newTasks.push(task)
      })
      this.tasks = newTasks
    }
  }
}
</script>

<style lang="sass" scoped>
@mixin flex-vender()
  display: flex
  display: -webkit-flex
  display: -moz-flex
  display: -ms-flex
  display: -o-flex
.task-list
  @include flex-vender
  flex-direction: column
  align-items: center
  &__item
    width: 270px
    text-align: left
    &.checked
      color: #85a6c6
</style>
