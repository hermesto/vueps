<template lang="pug">
  #app
    PmHeader
    pm-notification(v-show="showNotification")
      p(slot="body") No se encontraron resultados
    pm-loader(v-show="isLoading")
    section.section(v-show="!isLoading")
      nav.nav.has-shadow
        .container
          input.input.is-large(type="text", placeholder="Buscar Canciones", v-model="searchQuery")
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is-danger.is-large &times;
      .container
        p
          small {{ searchMessage }}
      .container.results
        .columns.is-multiline
          .column.is-one-quarter(v-for="t in tracks")
            pm-track(:class="{ 'is-active': t.id === selectedTrack }", v-bind:track="t", @select="setSelectedTrack")
    PmFooter
</template>
<script>
import trackService from '@/services/track'
import PmHeader from '@/components/layout/header.vue'
import PmFooter from '@/components/layout/footer.vue'
import PmTrack from '@/components/Track.vue'
import PmLoader from '@/components/shared/Loader.vue'
import PmNotification from '@/components/shared/Notification.vue'

export default {
  name: 'app',
  components: { PmFooter, PmHeader, PmTrack, PmLoader, PmNotification },
  data () {
    return {
      searchQuery: '',
      tracks: [],
      isLoading: false,
      selectedTrack: '',
      showNotification: false
    }
  },
  created () {
    console.log('created')
  },
  mounted () {
    console.log('mounted')
  },
  computed: {
    searchMessage () {
      return `Encontrados ${this.tracks.length}`
    }
  },
  watch: {
    showNotification () {
      if (this.showNotification) {
        setTimeout(() => {
          this.showNotification = false
        }, 3000)
      }
    }
  },
  methods: {
    search () {
      if (!this.searchQuery) { return }
      this.isLoading = true
      trackService.search(this.searchQuery)
        .then(res => {
          console.log(res)
          this.showNotification = res.tracks.total === 0
          this.tracks = res.tracks.items
          this.isLoading = false
        })
    },
    setSelectedTrack (id) {
      this.selectedTrack = id
    }
  }

}

</script>

<style lang="scss">
  @import './scss/main.scss';
  .results {
    margin-top: 50px
  }
  .is-active {
    border: 3px #23d160 solid;
  }
</style>
