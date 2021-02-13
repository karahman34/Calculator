<template>
  <div class="calculator">
    <div class="output">
      <div class="previous">{{ previousOutput }}</div>
      <div class="current">{{ current || '0' }}</div>
    </div>

    <button class="span-two operator-medium-character" data-action="all-clear">
      AC
    </button>
    <button class="operator-medium-character" data-action="delete">DEL</button>
    <button data-action="operator">%</button>
    <button data-action="number">7</button>
    <button data-action="number">8</button>
    <button data-action="number">9</button>
    <button data-action="operator">/</button>
    <button data-action="number">4</button>
    <button data-action="number">5</button>
    <button data-action="number">6</button>
    <button data-action="operator">*</button>
    <button data-action="number">1</button>
    <button data-action="number">2</button>
    <button data-action="number">3</button>
    <button data-action="operator">-</button>
    <button data-action="number">0</button>
    <button data-action="number">.</button>
    <button data-action="equal">=</button>
    <button data-action="operator">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: '',
      previous: '',
      operator: ''
    }
  },

  computed: {
    previousOutput() {
      return this.operator.length
        ? `${this.previous} ${this.operator}`
        : this.previous
    }
  },

  mounted() {
    const calcButtons = this.$el.querySelectorAll('button')
    for (let index = 0; index < calcButtons.length; index++) {
      calcButtons[index].addEventListener('click', this.buttonClickHandler)
    }
  },

  methods: {
    buttonClickHandler(e) {
      const button = e.target
      const action = button.dataset.action
      const btnContent = button.textContent

      if (action === 'number') {
        this.appendNumber(btnContent)
      } else if (action === 'delete') {
        this.remove()
      } else if (action === 'all-clear') {
        this.current = ''
        this.previous = ''
        this.operator = ''
      } else if (action === 'operator') {
        if (
          this.previous.length &&
          this.current.length &&
          this.operator.length
        ) {
          this.calculate()
          this.previous = this.current
          this.operator = btnContent
          this.current = ''
        } else if (this.current.length) {
          this.previous = this.current
          this.current = ''
          this.operator = btnContent
        } else if (
          this.previous.length &&
          this.operator.length &&
          !this.current.length
        ) {
          this.operator = btnContent
        }
      } else {
        this.calculate()
      }
    },
    appendNumber(number) {
      if (number === '.' && this.current.includes('.')) return
      this.current = this.current.toString() + number.toString()
    },
    remove() {
      if (this.current.length) {
        this.current = this.current.slice(0, -1)
      } else if (this.operator.length) {
        this.operator = ''
      } else {
        this.previous = this.previous.slice(0, -1)
      }
    },
    calculate() {
      if (
        !this.previous.length ||
        !this.operator.length ||
        !this.current.length
      )
        return

      const prev = parseFloat(this.previous)
      const curr = parseFloat(this.current)
      const history = {
        calculation: `${this.previous} ${this.operator} ${this.current} =`,
        result: null
      }

      switch (this.operator) {
        case '+':
          this.current = prev + curr
          break

        case '-':
          this.current = prev - curr
          break

        case '*':
          this.current = prev * curr
          break

        case '/':
          this.current = prev / curr
          break

        default:
          this.current = prev % curr
          break
      }

      this.operator = ''
      this.previous = ''
      this.current = this.current.toString()

      history.result = this.current
      this.$emit('calculated', history)
    }
  }
}
</script>
