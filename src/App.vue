<template lang="pug">
  #app
    .columns.is-mobile
      .column.is-half.is-offset-one-quarter
        .todo-container
          h1.todo-title
            strong TODO LIST
          p User: {{ name }}
          fieldset.todo-form
            legend Add new task
            p 
              input(type="text", placeholder="Title", v-model="newTask.title")
            p 
              input(type="number", placeholder="Hours worked", v-model="newTask.times")
            p
              button(@click="addTask") Save
              button(@click="cancel") Cancel
          p(v-show="!tasks.length") There isn't tasks yet ...
          div
            ol.todo-list
              li(v-for="(t, i) in tasks") 
                span {{ t.title }} - {{ t.times }}
                span(@click="removeTask(i)")  ❌
            hr
            strong Total: {{ totalTime }}
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
     name: 'Hector Flores',
     newTask: {
      title: '',
      times: 0
     },
     tasks: []
    }
  },
  created() {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  methods: {
    addTask() {
      if (this.newTask.title != '' && this.newTask.time != 0) {

        this.tasks.push(this.newTask)

        localStorage.setItem('tasks', JSON.stringify(this.tasks))

        this.newTask.title = ''
        this.newTask.time = 0 
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks)) 
    },
    cancel () {
      this.newTask.title = ''
      this.newTask.times = 0
    }
  },
  computed: {
    totalTime () {
      return this.tasks.length ? this.tasks.reduce((a, b) => return parseInt(a.times) + parseInt(b.times) ) : 0
    }
  }
}
</script>

<style lang="scss">
@import './scss/main.scss';

</style>
