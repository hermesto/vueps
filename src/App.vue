<template lang="pug">
  #app
    section.section
      nav.nav.has-shadow
        .container
          input.input.is-large(type="text", placeholder="buscar canciones",v-model="searchQuery")
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is-danger.is-large &times;
          p
            small {{ searchMessage }}

      .container.results
        .columns
          .column(v-for="t in tracks") {{ t.name }} - {{ t.artist }}
</template>

<script>
const tracks = [
  { name: 'Muchacha', artist: 'Luis Alberto Spinneta' },
  { name: 'Hoy aca en el baile', artist: 'H' },
  { name: 'La chachalaca del otro piso', artist: 'anonimo XD' }
]

export default {
  name: 'app',
  data () {
    return {
      searchQuery: '',
      tracks: []
    }
  },
  computed: {
    searchMessage () {
      return `Encontrados ${this.tracks.length}`
    }
  },
  methods: {
    search () {
      this.tracks = tracks
      console.log(this.searchQuery)
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
