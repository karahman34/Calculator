<template>
  <div id="app">
    <h1 class="app-title">Calculator</h1>

    <!-- The Calculator -->
    <calculator @calculated="calculatedHandler"></calculator>

    <!-- The History -->
    <history v-if="histories.length" :histories="histories"></history>
  </div>
</template>

<script>
import Calculator from '@/components/Calculator'
import History from '@/components/History'

export default {
  name: 'App',

  components: {
    Calculator,
    History
  },

  data() {
    return {
      histories: []
    }
  },

  methods: {
    calculatedHandler(history) {
      this.histories.push(history)
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/sass/app.scss';
</style>
