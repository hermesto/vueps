<template lang="pug">
  #app
    PmHeader
    section.section
      nav.nav.has-shadow
        .container
          input.input.is-large(type="text", placeholder="Buscar Canciones", v-model="searchQuery")
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is-danger.is-large &times;
      .container
        p
          small {{ searchMessage }}
      .container.results
        .columns(v-for="t in tracks")
          .column
            | {{ t.name }} - {{ t.artists[0].name }}
    PmFooter
</template>
<script>
import trackService from './services/track'
import PmHeader from './components/layout/header.vue'
import PmFooter from './components/layout/footer.vue'

export default {
  name: 'app',
  components: { PmFooter, PmHeader },
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
      if (!this.searchQuery) { return }
      trackService.search(this.searchQuery)
        .then(res => {
          this.tracks = res.tracks.items
          console.log(res)
        })
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
