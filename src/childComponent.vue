<template lang="pug">
  div
    h1 Este es un componente hijo
    LocalComponent
</template>
<script>
import localComponent from './localComponent.vue'

export default {
  components: {
    LocalComponent: localComponent
  }
}
</script>
