<template>
  <div class="main">
      <h1>{{ text }}</h1>
      <div class="error" v-if='error'>{{error}}</div>
      <input type="number" v-model.number="op1">
      <input type="number" v-model.number="op2">
      ={{ result }}
      <div class="buttons">
        <button v-for="operator in operators" v-bind:key = "operator" v-bind:title = "operator"  @click = "calculate(operator)">{{operator}}</button>
      </div>
    <div class="checkbox">
    <label>
      <input type="checkbox" v-model="checked">
      {{message}}
    </label>
    <div class="keyboard" v-if = "checked == true">
      <template >
          <button v-for = "keybtn in keybtns" v-bind:key = "keybtn" @click = "dataInput(keybtn)">{{keybtn}}</button>
          <button v-show = "clearBtn" v-bind:key = "clearBtn" @click = "clear()">{{clearBtn}}</button>
      </template>
      <div class = "radio">
        <input type = "radio" id = "op1" value = "op1" name = "radio" v-model = "picked">
        <label for = "op1">Операнд 1</label>
        <input type = "radio" id = "op2" value = "op2" name = "radio" v-model = "picked">
        <label for = "op2">Операнд 2</label>
      </div>
    </div>
    <div class = "logs" v-for = "(log, id) in logs" v-bind:key = "id"> {{ log }} </div>
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
    error: '',
    operators: ['+', '-', '*', '/', 'pow', 'int'],
    logs: {},
    message: 'Отобразить экранную клавиатуру',
    keybtns: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'],
    checked: false,
    clearBtn: 'clear',
    digits1: [],
    digits2: [],
    picked: ''
  }),
  methods: {
    calculate (operation) {
      this.error = ''
      switch (operation) {
        case '+': this.sum(); break
        case '-': this.sub(); break
        case '*': this.mult(); break
        case '/': this.div(); break
        case 'pow': this.pow(); break
        case 'int': this.int(); break
      }
      // this.logs[Date.now()] = `${this.op1}${operation}${this.op2}=${this.result}`
      const key = Date.now()
      const value = `${this.op1}${operation}${this.op2}=${this.result}`
      this.$set(this.logs, key, value)
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
    },
    dataInput (keybtn) {
      switch (this.picked) {
        case 'op1':
          this.digits1.push(keybtn)
          this.op1 = Number(this.digits1.join('')); break
        case 'op2':
          this.digits2.push(keybtn)
          this.op2 = Number(this.digits2.join('')); break
      }
    },
    clear () {
      switch (this.picked) {
        case 'op1':
          this.digits1.pop()
          this.op1 = Number(this.digits1.join('')); break
        case 'op2':
          this.digits2.pop()
          this.op2 = Number(this.digits2.join('')); break
      }
    }
  }
}

</script>
