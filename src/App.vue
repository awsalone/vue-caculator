<template>
  <div id="app">
    <div class="caculator">
      <div class="result" style="grid-area:result">{{equation}}</div>
      <button style="grid-area:ac" @click="clear()">AC</button>
      <button style="grid-area:plus-minus" @click="togglecal()">±</button>
      <button style="grid-area:percent" @click="percentcal()">%</button>
      <button style="grid-area:divide" @click="append('÷')">÷</button>
      <button style="grid-area:number-7" @click="append(7)">7</button>
      <button style="grid-area:number-8" @click="append(8)">8</button>
      <button style="grid-area:number-9" @click="append(9)">9</button>
      <button style="grid-area:multiply" @click="append('×')">×</button>
      <button style="grid-area:number-4" @click="append(4)">4</button>
      <button style="grid-area:number-5" @click="append(5)">5</button>
      <button style="grid-area:number-6" @click="append(6)">6</button>
      <button style="grid-area:minus" @click="append('-')">－</button>
      <button style="grid-area:number-1" @click="append(1)">1</button>
      <button style="grid-area:number-2" @click="append(2)">2</button>
      <button style="grid-area:number-3" @click="append(3)">3</button>
      <button style="grid-area:plus" @click="append('+')">＋</button>
      <button style="grid-area:number-0" @click="append(0)">0</button>
      <button style="grid-area:dot" @click="append('.')">.</button>
      <button style="grid-area:equal" @click="caculate()">=</button>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      equation: '0', // result显示
      isStarted: false, // 开始
      isdot: false, // 小数点存在
      isOperator: false, // 计算符号
      iscaculated: false // 计算完成状态
    }
  },
  methods: {
    // 是否为运算符 true 是
    isOperatorStatus (num) {
      return ['+', '-', '×', '÷'].indexOf(num) !== -1
    },
    append (num) {
      // start
      if (this.equation === '0' && !this.isOperatorStatus(num)) {
        if (num === '.') {
          this.equation += num
          this.isdot = true
          this.isOperator = true
        } else if (num == '0') {
          return
        }
        else {
          this.equation = num
        }
        this.isStarted = true
        return
      }
      // number
      if (!this.isOperatorStatus(num)) {
        if ((this.isdot && num === '.') || (this.isOperator && num === '.')) {
          return
        } else if (num === '.') {
          this.isdot = true
          this.isOperator = true
          this.iscaculated = false
        } else {
          this.isOperator = false
        }
        if (this.iscaculated) {
          this.equation = num
          this.iscaculated = false
          return
        }
        this.equation += '' + num
      }
      // operater
      if (this.isOperatorStatus(num)) {
        if (!this.isOperator) {
          this.equation += num
          this.isdot = false
          this.isOperator = true
          this.iscaculated = false
        } else {
          let result = this.equation
          this.equation = result.replace(result.charAt(result.length - 1), num)

        }

      }
    },
    // =
    caculate () {
      if (this.isOperator || this.isdot) {
        this.equation = this.equation.substring(0, this.equation.length - 1)
        this.isdot = false
        this.isOperator = false
        this.isStarted = true
        this.iscaculated = true
      }
      let result = this.equation.replace(new RegExp('×', 'g'), '*').replace(new RegExp('÷'), '/')
      this.equation = parseFloat(eval(result).toFixed(8)).toString()
      this.isdot = false
      this.isStarted = true
      this.iscaculated = true
    },
    // ac
    clear () {
      this.equation = '0'
      this.isOperator = false
      this.isdot = false
      this.isStarted = false

    },
    // +_
    togglecal () {
      if (this.isOperator || !this.isStarted) {
        return
      }
      this.equation = this.equation + '*-1'
      this.caculate()
    },
    // %
    percentcal () {
      if (this.isOperator || !this.isStarted) {
        return
      }
      this.equation = this.equation + '* 0.01'
      this.caculate()
    }
  }
}
</script>

<style lang='scss'>
body {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #eee;
  #app {
    .caculator {
      --button-width: 80px;
      --button-height: 80px;
      display: grid;
      grid-template-columns: repeat(4, var(--button-width));
      grid-template-rows: repeat(6, var(--button-height));
      box-shadow: -10px -10px 16px -10px rgba(255, 255, 255, 1),
        10px 10px 16px -10px rgba(0, 0, 0, 0.5);
      grid-template-areas:
        'result result result result '
        'ac plus-minus percent divide'
        'number-7 number-8 number-9 multiply'
        'number-4 number-5 number-6 minus'
        'number-1 number-2 number-3 plus'
        'number-0 number-0 dot equal';
      padding: 12px;

      border-radius: 5px;
      button {
        outline: none;
        margin: 8px;
        padding: 0;
        border: 0;
        font-family: Helvetica;
        font-size: 24px;
        font-weight: normal;
        color: #999;
        border-radius: calc(var(--button-height) / 2);
        background: linear-gradient(
          132deg,
          rgba(230, 230, 230, 1) 0%,
          rgba(246, 246, 246, 1) 100%
        );
        box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1),
          4px 4px 10px -8px rgba(0, 0, 0, 0.3);
        &:active {
          box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1) inset,
            4px 4px 10px -8px rgba(0, 0, 0, 0.3) inset;
        }
      }
      .result {
        text-align: right;
        line-height: var(--button-height);
        font-family: Helvetica;
        padding: 0 20px;
        color: #666;
        font-size: 42px;
        overflow-x: auto;
        overflow-y: hidden;
      }
    }
  }
}
</style>
