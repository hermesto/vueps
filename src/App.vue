<template lang="pug">
  #app
    .columns
      .column {{ name }}
        h1.title.has-background-info(v-if="!comprobarLista") las horas que debes invertir para ser un poco menos ignorante son: {{ totalTime }}
        h1.title.has-background-warning(v-else) No hay tareas que mostrar
      .column
        h1 Agrega una nueva Tarea
        input.input(v-model="newTask.title")
        span(v-show="mensajeTitle").tag.is-warning.is-large El texto no es valido
        input.input(v-model="newTask.time")
        span(v-show="mensajeTime").tag.is-warning.is-large ingresa un número valido
        br
        br
        button.button(@click="addTask") Agregar tarea
        button.button.is-danger(@click="cancelTask") cancelar Tarea
    .columns
      .column
        h1 Tus Tareas Pendientes son
        ul
          li(v-for="(task, index) in tasks")
            span.tag.is-success {{index}} {{ task.title }}, Tiempo Estimado {{ task.time }} horas
              button.delete(@click="deleteTask($event, index)")
</template>

<script>

export default {
  name: 'app',
  created () {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  data () {
    return {
      name: 'HERMESTO',
      tasks: [
        {
          title: 'titulo 1',
          time: 2
        },
        {
          title: 'titulo 2',
          time: 5
        }
      ],
      newTask: {
        title: '',
        time: 0
      },
      mensajeTitle: false,
      mensajeTime: false
    }
  },
  computed: {
    totalTime () {
      let total = 0
      for (let i = 0; i < this.tasks.length; i++) {
        total += parseInt(this.tasks[i].time)
      }
      return total
    },
    comprobarLista () {
      if (this.tasks.length === 0) {
        return true
      } else {
        return false
      }
    }
  },
  methods: {
    addTask () {
      if (this.newTask.title.trim().length === 0) {
        this.mensajeTitle = true
        return false
      } else {
        this.mensajeTitle = false
      }
      if (parseInt(this.newTask.time) > 0) {
        this.mensajeTime = false
      } else {
        this.mensajeTime = true
        console.log('falio')
        return false
      }
      let tmpTask = {
        title: '',
        time: 0
      }
      tmpTask.title = this.newTask.title
      tmpTask.time = this.newTask.time
      this.newTask.title = ''
      this.newTask.time = 0
      this.tasks.push(tmpTask)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    cancelTask () {
      this.newTask.title = ''
      this.newTask.time = 0
    },
    deleteTask ($event, index) {
      this.tasks.splice(index, 1)
    }
  }

}

</script>

<style lang="scss">
  @import './scss/main.scss';
  .results {
    margin-top: 50px
  }
</style>
