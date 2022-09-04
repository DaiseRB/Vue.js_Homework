<template>
  <div class="main">
    <h1>{{ text }}</h1>
    <div class="error" v-if='error'>{{error}}</div>
    <input type="number" v-model.number="op1">
    <input type="number" v-model.number="op2">
    ={{ result }}
    <div class="keyboard">
      <button @click="calculate('+')">+</button>
      <button @click="calculate('-')">-</button>
      <button @click="calculate('*')">*</button>
      <button @click="calculate('/')">/</button>
      <button @click="calculate('pow')">pow</button>
      <button @click="calculate('int')">int</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MultiCalc',
  data: () => ({
    text: 'Calculator',
    op1: 0,
    op2: 0,
    result: 0,
    error: ''
  }),
  methods: {
    calculate (operation) {
      this.error = ''
      switch (operation) {
        case '+': return this.sum()
        case '-': return this.sub()
        case '*': return this.mult()
        case '/': return this.div()
        case 'pow': return this.pow()
        case 'int': return this.int()
      }
    },
    sum () {
      const { op1, op2 } = this
      this.result = op1 + op2
    },
    sub () {
      const { op1, op2 } = this
      this.result = op1 - op2
    },
    mult () {
      const { op1, op2 } = this
      this.result = op1 * op2
    },
    div () {
      const { op1, op2 } = this
      if (op2 === 0) {
        this.error = 'На ноль делить нельзя!'
        return
      }
      this.result = op1 / op2
    },
    pow () {
      const { op1, op2 } = this
      this.result = Math.pow(op1, op2)
    },
    int () {
      const { op1, op2 } = this
      if (op2 === 0) {
        this.error = 'На ноль делить нельзя!'
        return
      }
      this.result = Math.floor(op1 / op2)
    }
  }
}
</script>
