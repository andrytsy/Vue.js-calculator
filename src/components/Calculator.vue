<template>
  <div class="calculator">
    <div class="display">{{result || current}}</div>
    <div @click="clear" class="btn operator">C</div>
    <div @click="sign" class="btn operator">+/-</div>
    <div @click="percent" class="btn operator">%</div>
    <div @click="divide" class="btn operator">/</div>
    <div @click="append('7')" class="btn">7</div>
    <div @click="append('8')" class="btn">8</div>
    <div @click="append('9')" class="btn">9</div>
    <div @click="times" class="btn operator">*</div>
    <div @click="append('4')" class="btn">4</div>
    <div @click="append('5')" class="btn">5</div>
    <div @click="append('6')" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>
    <div @click="append('1')" class="btn">1</div>
    <div @click="append('2')" class="btn">2</div>
    <div @click="append('3')" class="btn">3</div>
    <div @click="add" class="btn operator">+</div>
    <div @click="append('0')" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn operator">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: '0',
      current: '0',
      result: false,
      operator: null,
      operatorClicked: false,
      isNeedCalc: true
    }
  },
  methods: {
    clear() {
      this.current = '0';
      this.previous = '0';
      this.result = false;
      this.operator = null;
      this.operatorClicked = false;
      this.isNeedCalc = true;
    },
    sign() {
      if (this.current !== '0') {
        this.current = this.current.charAt(0) === '-' ?
          this.current.slice(1) : `-${this.current}`;
      }
    },
    percent() {
      if (this.previous)
        this.current = `${parseFloat(this.current)*parseFloat(this.previous) / 100}`;
      else
        this.current = '0';
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '0';
        this.operatorClicked = false; 
      }
      this.current = this.current !== '0' || number == '.' ? 
        `${this.current}${number}` : number;
      this.result = false;
    },
    dot() {
      if (this.current.indexOf('.') === -1)
        this.append('.');
    },
    setPrevious() {
      if (this.result)
        this.previous = this.result;
      else
        this.previous = this.current;

      this.operatorClicked = true;
      this.isNeedCalc = true;
    },
    divide() {
      if (this.isNeedCalc)
        this.equal();
      this.operator = (a, b) => b / a;
      this.setPrevious();
    },
    times() {
      if (this.isNeedCalc)
        this.equal();
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    minus() {
      if (this.isNeedCalc)
        this.equal();
      this.operator = (a, b) => b - a;
      this.setPrevious();
    },
    add() {
      if (this.isNeedCalc)
        this.equal();
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    equal() {
      if (this.operator && this.previous) {
        if (this.result)
          this.previous = this.result;

        this.isNeedCalc = false;
        this.operatorClicked = true;
        this.result = `${this.operator(
          parseFloat(this.current),
          parseFloat(this.previous)
        )}`;
      }
    }
  } 
}
</script>

<style scoped>
.calculator {
  width: 500px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  margin: 50px auto;
  text-align: center;
  box-shadow: 5px 5px 30px #333;
  -moz-user-select: none;
  -khtml-user-select: none;
  user-select: none;
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
  text-align: right;
  padding-right: 10px;
}

.zero {
  grid-column: 1 / 3; 
}

.btn {
  background-color: #eee;
  border: 1px solid #333;
}

.btn:hover {
  border: 1px solid #fff;
}
.btn:active {
  box-shadow: 0px 0px 3px #555 inset; 
  border: 1px solid #333;
}
 
.operator {
  background-color: orange;
  color: white;
}
</style>
