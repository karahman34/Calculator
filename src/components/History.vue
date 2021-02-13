<template>
  <div class="history">
    <div class="header">
      <i class="mdi mdi-history"></i>
      <span>History</span>
    </div>

    <div class="content">
      <div class="content-item" v-for="(history, i) in histories" :key="i">
        <div class="calculation">{{ history.calculation }}</div>
        <div class="result">{{ history.result }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    histories: {
      type: Array,
      required: true
    }
  },

  watch: {
    histories() {
      this.$nextTick(() => {
        this.updateScroll()
      })
    }
  },

  mounted() {
    const calculator = document.querySelector('.calculator')
    this.$el.style.width = calculator.scrollWidth + 'px'
  },

  methods: {
    updateScroll() {
      const content = this.$el.querySelector('.content')
      content.scrollTop = this.$el.scrollHeight
    }
  }
}
</script>
