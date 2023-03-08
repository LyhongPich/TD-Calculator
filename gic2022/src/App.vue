<template>
  <div id="app">
    <div class="container">
      <table>
        <tr>
          <td colspan="5">
            <div id="screen">
              <span id="screen_top">M={{ memory }}</span>
              <div id="screen_bottom">
                <!-- v-text is a directive that is used to replace the content of HTML tag with private data -->
                <!-- It will update the content automatically when data is changed. It is called data reactive -->
                <span v-show="isVisible" id="operand1">{{ inputNumber }}</span>
                <span v-show="isHidden" id="operator">{{ operation }}</span>
                <span v-show="isHidden" id="operand2">{{ operand2 }}</span>
              </div>
              <!-- <span id="screen_bottom">0</span> -->
            </div>
          </td>
        </tr>
        <tr>
          <td>
            <button v-on:click="memoryClear()" type="button" class="btn btn-warning">MC</button>
          </td>
          <td>
            <button v-on:click="memoryRecall()" type="button" class="btn btn-warning">MR</button>
          </td>
          <td>
            <button v-on:click="memoryOperation('-')" type="button" class="btn btn-warning">M-</button>
          </td>
          <td>
            <button v-on:click="memoryOperation('+')" type="button" class="btn btn-warning">M+</button>
          </td>
          <td>
            <button v-on:click="clearAll()" type="button" class="btn btn-light">
              <i class="fa fa-long-arrow-right" aria-hidden="true"></i>
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <button v-on:click="showNumber(7)" type="button" class="btn btn-light">7</button>
          </td>
          <td>
            <button v-on:click="showNumber(8)" type="button" class="btn btn-light">8</button>
          </td>
          <td>
            <button v-on:click="showNumber(9)" type="button" class="btn btn-light">9</button>
          </td>
          <td>
            <button @click="changeOperation('/')" type="button" class="btn btn-secondary">÷</button>
          </td>
          <td>
            <button v-on:click="PosOrNeg()" type="button" class="btn btn-light">+/-</button>
          </td>
        </tr>
        <tr>
          <td>
            <button v-on:click="showNumber(4)" type="button" class="btn btn-light">4</button>
          </td>
          <td>
            <button v-on:click="showNumber(5)" type="button" class="btn btn-light">5</button>
          </td>
          <td>
            <button v-on:click="showNumber(6)" type="button" class="btn btn-light">6</button>
          </td>
          <td>
            <button v-on:click="changeOperation('*')" type="button" class="btn btn-secondary">x</button>
          </td>
          <td>
            <button v-on:click="changeOperation('-')" type="button" class="btn btn-secondary">-</button>
          </td>
        </tr>
        <tr>
          <td>
            <button v-on:click="showNumber(1)" type="button" class="btn btn-light">1</button>
          </td>
          <td>
            <button v-on:click="showNumber(2)" type="button" class="btn btn-light">2</button>
          </td>
          <td>
            <button v-on:click="showNumber(3)" type="button" class="btn btn-light">3</button>
          </td>
          <td rowspan="2">
            <button @click="changeOperation('+')" type="button" class="btn btn-secondary long-btn">+</button>
          </td>
          <td rowspan="2">
            <button v-on:click="calculateResult()" type="button" class="btn btn-primary long-btn">=</button>
          </td>
        </tr>
        <tr>
          <td>
            <button v-on:click="clearNum()" type="button" class="btn btn-danger">C</button>
          </td>
          <td>
            <button v-on:click="showNumber(0)" type="button" class="btn btn-light">0</button>
          </td>
          <td>
            <button type="button" class="btn btn-light">.</button>
          </td>
        </tr>
      </table>
    </div>
    <div class="alert alert-danger" id="message_panel" role="alert">
      something wrong here
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data() {
    return {
      // This is the private data section which can be used inside this component
      isVisible: true, // For showing first operand and also the output
      isHidden: false, // Hide the operation since there is no operation yet
      inputNumber: 0, // First operand
      memory: 0, // Value store in memory
      sign: '+', // For pos or neg value by default it is positive 
      operand2: null, // Second operand
      operation: '', // Operation for 2 operands
    };
  },
  methods: {
    showNumber(number) {
      // Assign number when user click to the inputNumber data
      // To access private data from methods, use (this.)
      if(this.operation == '') { // If there is no operation yet display on the first operand
        this.inputNumber = (this.inputNumber*10) + number;
        console.log("Operand1: "+this.inputNumber);
      }
      else{ // Operation detected meaning there exist second operand
        this.operand2 = (this.operand2*10) +number;
        console.log("Operand2: "+this.operand2);
      }
    },
    memoryRecall(){ // MR
      if(this.operation == '') { // If no operation get memory to display as first operand
        this.inputNumber = this.memory;
      }
      else{ // Get memory to display as second operand
        this.operand2 = this.memory;
      }
    },
    memoryOperation(op){ // M+ or M-
      if(op == '+') { // M+ Increase number input by memory value
        this.memory += this.inputNumber;
      }
      else if(op == '-') { // M- Decrease number input by memory value
        this.memory -= this.inputNumber;
      }
      console.log("Memory operation: "+op);
      console.log("Memory value: "+this.memory);
    },
    memoryClear(){ // MC clearing value from memory (make it default value = 0)
      this.memory = 0;
    },
    PosOrNeg(){ // Sign value or not (+/-)
      if(this.sign == '+') { // If value has sign(+) then change to sign(-) when triggered
        this.sign = '-';
        if(this.operation == '') { // Change first operand to negative value
          this.inputNumber = -this.inputNumber;
        }
        else{ // Change second operand to negative value
          this.operand2 = -this.operand2;
        }
      }
      else if(this.sign == '-') { // If value has sign(-) change to opposite sign when trigger
        this.sign = '+';
        if(this.operation == '') { // Making first operand become positive value 
          this.inputNumber = Math.abs(this.inputNumber);
        }
        else{ // Second operand become negative value
          this.operand2 = Math.abs(this.operand2);
        }
      }
    },
    clearNum() { // C clear number
      this.isHidden = false; // Hide operation and second operand
      if(this.operation == '') { // If there is no operation clear the first operand
        this.inputNumber = 0;
        console.log("Clear operand1");
      }
      else{ // If there exist any operations clear the second operand
        this.operand2 = null;
        console.log("Clear operand2");
      }
      this.operation = ''; // Clear operation after clearing any operand
      this.sign = '+'; // After clearing operand number should be positive
    },
    clearAll() { // Clear everything (except memory)
      this.isHidden = false; // Hide operation and second operand
      this.inputNumber = 0;
      this.operand2 = null;
      this.operation = '';
      this.sign = '+';
      console.log("Clear all numbers");
    },
    changeOperation(op) { // Display operation when operation button is triggered
      this.isHidden = true;
      this.operation = op;
      console.log("Operation used: "+this.operation);
    },
    calculateResult() { // = Show result of the operation
      this.isHidden = false; // Hide operation and second operand since first operand is the result

      if(this.operation == '+') { // Addition operation
        this.inputNumber += this.operand2;
      }
      else if(this.operation == '-'){ // Subtraction operation
        this.inputNumber -= this.operand2;
      }
      else if(this.operation == '*') { // Multiplication operation
        this.inputNumber *= this.operand2;
      }
      else if(this.operation == '/') { // Division operation
        this.inputNumber /= this.operand2;
      }
      console.log("Result is: "+this.inputNumber);
      this.operation = '';
      this.operand2 = null;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  margin-top: 10em;
  width: 300px;
  border: 1px solid black;
  padding-top: 20px;
  padding-bottom: 20px;
}
table {
  border-spacing: 7px;
  border-collapse: separate;
}
#screen {
  border: 1px solid black;
  padding: 7px;
  width: 100%;
  height: 4em;
}
#screen_top {
  display: block;
  font-size: 0.8rem;
}
#screen_bottom {
  font-size: 1.8rem;
  display: block;
  text-align: right;
}
#operand2 {
  background-color: skyblue;
}
#operator {
  background-color: rgb(82, 239, 87);
}
.button-row {
  display: flex;
  justify-content: space-between;
}
button {
  width: 45px;
}
.long-btn {
  display: inline-block;
  height: 80px;
}

/* Message panel */
#message_panel {
  width: 300px;
  margin-top: 1em;
  display: none;
  margin-left: auto;
  margin-right: auto;
}
</style>
