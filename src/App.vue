<template>
  <div>
    <input class="field" type="text" :value="this.showedPrompt"/>
    <div class="items-1">
      <button class="item" type="button" @click="handleNumberClick('7')">7</button>
      <button class="item" type="button" @click="handleNumberClick('8')">8</button>
      <button class="item" type="button" @click="handleNumberClick('9')">9</button>

      <button class="item action" type="button" @click="deleteItem">DEL</button>

      <button class="item" type="button" @click="handleNumberClick('4')">4</button>
      <button class="item" type="button" @click="handleNumberClick('5')">5</button>
      <button class="item" type="button" @click="handleNumberClick('6')">6</button>

      <button class="item action" type="button" @click="handleOperationClick('+')">+</button>

      <button class="item" type="button" @click="handleNumberClick('1')">1</button>
      <button class="item" type="button" @click="handleNumberClick('2')">2</button>
      <button class="item" type="button" @click="handleNumberClick('3')">3</button>

      <button class="item action" type="button" @click="handleOperationClick('-')">-</button>

      <button class="item" type="button" @click="handleNumberClick('.')">&middot;</button>
      <button class="item" type="button" @click="handleNumberClick('0')">0</button>

      <button class="item action" type="button" @click="handleOperationClick('/')">/</button>
      <button class="item action" type="button" @click="handleOperationClick('*')">*</button>
    </div>
    <div class="items-2">
      <button class="item reset" type="button" @click="deleteAll">RESET</button>
      <button class="item action" type="button" @click="handleCalculateClick">=</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstNumber: '',
      secondNumber: '',
      action: '',
      step: 0,
    }
  },
  methods: {
    handleNumberClick(number) {
      if (this.step == 0) {
        if ((number == '0' && this.firstNumber == '0')) {
          return;
        }
        if ((number == '.' && (this.firstNumber.length == 0 || this.firstNumber.indexOf('.') != -1))) {
          return;
        }
        this.firstNumber = this.firstNumber + number;
      } 
      if (this.step == 1) {
        if ((number == '0' && this.secondNumber == '0')) {
          return;
        }
        if ((number == '.' && (this.secondNumber.length == 0 || this.secondNumber.indexOf('.') != -1))) {
          return;
        }
        this.secondNumber = this.secondNumber + number;
      }
    },

    handleOperationClick(action) {
      if (this.step == 0) {
        this.action = action;
        this.step = 1;
      } if (this.step == 1) {
        this.handleCalculateClick();
        this.action = action;
      }
    },

    handleCalculateClick() {
      if (this.step == 1) {
        const firstNumber = +this.firstNumber;
        const secondNumber = +this.secondNumber;
        if (this.action == '+') {
          this.firstNumber = (firstNumber + secondNumber).toString();
        } if (this.action == '-') {
          this.firstNumber = (firstNumber - secondNumber).toString();
        } if (this.action == '*') {
          this.firstNumber = (firstNumber * secondNumber).toString();
        } if (this.action == '/') {
          this.firstNumber = (firstNumber / secondNumber).toString();
        }
        this.step == 0;
        this.action = '';
        this.secondNumber = '';
      }
    },

    deleteAll() {
      this.firstNumber = '',
      this.secondNumber = '',
      this.action = '',
      this.step = 0
    },

    deleteItem() {
      if (this.step == 0) {
        this.firstNumber = this.firstNumber.slice(0, -1);
      } if (this.step == 1) {
        this.secondNumber = this.secondNumber.slice(0, -1);
      }
    }
  },
  computed: {
    showedPrompt() {
      return `${this.firstNumber} ${this.action} ${this.secondNumber}`;
    }
  }
}
</script>

<style>
.field {
  width: 230px;
  height: 30px;
  padding: 0;
  background-color: rgb(195, 215, 231);
  border:1px solid blue;
  margin-bottom: 10px;
  outline: none;
  border-radius: 10px;
}

.items-1 {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: 50px 50px 50px 50px;
  gap: 10px;
  margin: 0 0 10px;
}

.items-2 { 
  display: grid;
  grid-template-columns: 110px 110px;
  grid-template-rows: 50px;
  gap: 10px
}

.item {
  border: 1px solid green;
  background-color: rgb(195, 231, 195);
  font-size: medium;
  font-weight: 500;
  border-radius: 10px;
}

.action {
  background-color: rgb(231, 215, 195);
  border-color: orange;
}

.reset {
  background-color: rgb(195, 215, 231);
  border-color: blue;
}
</style>
