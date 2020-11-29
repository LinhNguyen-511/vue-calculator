<template>
  <div class="calculator">
    <div class="display">{{current || '0'}}</div>
    <!-- call function clear on click -->
    <div v-on:click='clear' class="button">C</div>
    <div @click='sign' class="button">+/-</div>
    <div @click='percent' class="button ">%</div>
    <div @click='divide' class="button operator">:</div>
    <div @click="append('7')" class="button">7</div>
    <div @click="append('8')" class="button">8</div>
    <div @click="append('9')" class="button">9</div>
    <div @click='times' class="button operator">x</div>
    <div @click="append('4')" class="button">4</div>
    <div @click="append('5')" class="button">5</div>
    <div @click="append('6')" class="button">6</div>
    <div @click='minus' class="button operator">-</div>
    <div @click="append('1')" class="button">1</div>
    <div @click="append('2')" class="button">2</div>
    <div @click="append('3')" class="button">3</div>
    <div @click='plus' class="button operator">+</div>
    <div @click="append('0')" class="button zero">0</div>
    <div @click="dot()" class="button">.</div>
    <div @click="equal()" class="button operator">=</div>
  </div>
</template>

<!--vue component -->
<script>
export default {
  data() {
    return {
      // .display shows current 
      previous: 0,
      current:'123',  
      operator: null,
      numOpClicked: 0
    }}, 
    methods: {
      //method clear set display to default
      clear() {
        // set the values to default
        this.current = ''
        this.previous = 0;
        this.numOpClicked = 0;
      }, 
      sign () {
        // ${} placeholder for template literal 
        // insert veriables and other expressions without
        // breaking the string
        // condition: if the no is negative, return the 
        // part behind the - (char 1) else return the 
        // no with -
        this.current = this.current.charAt(0) === '-' ?
          this.current.slice(1) : `-${this.current}`
      },
      percent() {
        this.current = `${parseFloat(this.current) / 100}`
      }, 
      append(number) {
        if(this.numOpClicked >= 1) {
          this.current = '';
        }
        this.current = `${this.current}${number}`;
      },
      dot() {
        // if there is no dot already, add .
        if(this.current.indexOf('.') === -1) {
          this.append('.');
        }
      },
      setUp() {
        this.numOpClicked++;
        if(this.numOpClicked > 1) {
          this.current = `${this.operator(this.previous, parseFloat(this.current))}`;
        }
        },
      divide(){
        this.operator = (a, b) => a / b;     
        this.setUp();
        this.previous = parseFloat(this.current);
      },
      times(){
        this.operator = (a, b) => a * b;
        this.setUp();
        this.previous = parseFloat(this.current);
      },
      minus(){
        this.operator = (a, b) => a - b;       
        this.setUp();
        this.previous = parseFloat(this.current);
      },
      plus(){
        this.operator = (a, b) => a + b;
        this.setUp();
        this.previous = parseFloat(this.current);
      },
      equal() {
        this.current = `${this.operator(this.previous, parseFloat(this.current))}`;
        this.numOpClicked = 0;
      }  
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  /* position the calculator in the middle */
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  /* at least 50px in height */
  grid-auto-rows: minmax(50px, auto);
}
.display {
  /* start at col1 end at col5 */
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}
.zero {
  grid-column: 1 / 3;
}
.button {
  background-color: #f2f2f2;
  border: 1px solid #999;
  cursor: pointer;
}
.button:active {
  transform: scale(0.95);
}
.operator {
  background-color: orange;
  color: white;
}
</style>
