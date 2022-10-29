<template>
  <div >
    <h1><i class="fa-solid fa-calculator"></i> {{ msg }}</h1>
    <br>
    <div class="container">
      <div class="size">
          <div class="card">
            <div class="card-header output ">

              {{ output || 0 }}

            </div>
            <div class="card-body p-0 m-0  ">
              <table class="table table-bordered m-0 ">
                <tbody>
                <tr>
                  <td class="otherColumn" @click="clearField">C</td>
                  <td class="otherColumn" @click="setNagativeOrPositive">+/-</td>
                  <td class="otherColumn" @click="calculatePercentage"><i class="fa-solid fa-percent"></i></td>
                  <td class="lastColumn" @click="processOutput('divide')"><i class="fa-solid fa-divide"></i></td>
                </tr>
                <tr>
                  <td class="otherColumn" @click="getNumber('7')">7</td>
                  <td class="otherColumn" @click="getNumber('8')">8</td>
                  <td class="otherColumn" @click="getNumber('9')">9</td>
                  <td class="lastColumn" @click="processOutput('multiply')"><i class="fa-solid fa-xmark"></i></td>
                </tr>
                <tr>
                  <td class="otherColumn" @click="getNumber('4')">4</td>
                  <td class="otherColumn" @click="getNumber('5')">5</td>
                  <td class="otherColumn" @click="getNumber('6')">6</td>
                  <td class="lastColumn" @click="processOutput('subtract')"><i class="fa-solid fa-minus"></i></td>
                </tr>
                <tr>
                  <td class="otherColumn" @click="getNumber('1')">1</td>
                  <td class="otherColumn" @click="getNumber('2')">2</td>
                  <td class="otherColumn" @click="getNumber('3')">3</td>
                  <td class="lastColumn" @click="processOutput('add')"><i class="fa-solid fa-plus"></i></td>
                </tr>
                <tr>
                  <td class="otherColumn" colspan="2" @click="getNumber('0')">0</td>
                  <td class="otherColumn" @click="getDot">.</td>
                  <td class="lastColumn" @click="updateOutput"><i class="fa-sharp fa-solid fa-equals"></i></td>
                </tr>
                </tbody>
              </table>

            </div>
          </div>

        </div>
     
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalculateNumber',
  props: {
    msg: String
  },
  data() {
    return {
      output: '',
      previousValue:null,
      operationFired:false,
    }
  },
  methods: {
    clearField() {
      this.output = '';
    },

    setNagativeOrPositive() {
      this.output = this.output[0] === '-' ? this.output.slice(1) : `-${this.output}`;
    },

    calculatePercentage() {
      this.output = parseFloat(this.output)/100;
    },

    getNumber(number){
      if (this.operationFired){
        this.output = '';
        this.operationFired = false;
      }
      this.output = `${this.output}${number}`;

    },
    getDot() {
      if (this.output.indexOf('.') === -1) {
        this.output = this.output + '.';
      }
    },

    processOutput(string){
      if (string == 'add'){
        this.operation = (a,b)=>{
          return parseFloat(a) + parseFloat(b);
        }
      }
      if (string == 'subtract'){
        this.operation = (a,b)=>{
          return parseFloat(a) - parseFloat(b);
        }
      }
      if (string == 'multiply'){
        this.operation = (a,b)=>{
          return parseFloat(a) * parseFloat(b);
        }
      }
      if (string == 'divide'){
        this.operation = (a,b)=>{
          return parseFloat(a) / parseFloat(b);
        }
      }
      this.previousValue = this.output;
      this.operationFired = true;
    },
    updateOutput(){
      this.output= `${this.operation(this.previousValue , this.output)}`;

    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.output {
  background-color: #333;
  color: #fff;
}

.lastColumn {
  background-color: orange;
  color: #fff;
}
.size {
    margin: 0 auto;
    width: 50%;
}
.lastColumn:active {
  background-color: #333;
  color: #fff;
}

.otherColumn:active {
  background-color: #f5f5f5;
}
</style>
